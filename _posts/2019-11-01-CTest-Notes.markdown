---
layout: post
title:  "CTest Notes"
date:   2019-11-01
categories: Java SpringBoot

---

### 注意：请在TMS项目中查询文中提到的代码
#### 完成题目后，上交前后台代码和运行截图
#### 1.导入Excel，查询数据，显示数据 
##### 该题目前还需要注意分页和倒序的实践
- 导入Excel：  

---

首先 **数据库连接**方面的参数在**application.properties**文件中设置,如下所示：  
```
spring.datasource.url =jdbc:oracle:thin:@IP:PORT:SERVERNAME  
spring.datasource.username = USERNAME
spring.datasource.password = USERPASSWORD
```
其次**创建数据库中的表格**，请在**navicat**（oracle可视化软件）中打开查询窗口，通过书写的sql语句来创建表格。如下所示：——应准备好常用的sql语句。
```
create table TEST_TAB_*
 (
  mid    VARCHAR2(32) default sys_guid(),
  bmname VARCHAR2(30),
  mname  VARCHAR2(30),
  mxb    NUMBER,
  msr    DATE,
  jg     VARCHAR2(30)
)
```
至此，数据库的准备工作结束了。开始进入正题......  

---

下面从**前端界面**开始，采用**layui**的前端框架。请从**templates**文件夹中的众多文件中寻找合适的轮子。  
使用按钮元素，主要代码如下。  
```
<button type="button" class="layui-btn layui-btn-radius" id="import">导入Excel</button>
//脚本
<script>
layui.use('upload', function () {
    var upload = layui.upload;
    //执行实例
    var uploadInst = upload.render({
        elem: '#import' //绑定元素-（button的ID）
        ,url: '/addManySt' //上传后段接口，见controller
        ,accept: 'file' //普通文件
        ,exts: 'xls|excel|xlsx' //只允许上传压缩文件
        , before: function (obj) {
            //可设置回显
        }
        , done: function (res) {
            console.log(res);
            return layer.msg('上传成功');
        }
        , error: function () {
            //请求异常回调
        }
    });
});
</script>
```
前端有关导入Excel部分的到此结束，下面终于开始最重要的部分了，即后台的具体实现。

---

后端的实现使用**springboot**的经典流程，即**controller**层->**service**层->**mapper**层。具体代码实现如下。  
```
//controller层
@RequestMapping(value="/addManySt",method=RequestMethod.POST)
	@ResponseBody
	public RestResult addManySt(@RequestParam(value = "file", required = false) MultipartFile file)
	{
		RestResult result=new RestResult();
		result.state=0;
		result.error="";
		result.message=new RestMessage();
		result.message.data=service.addManySt(file);
		return result;
	}
//service层
//读取MultipartFile的excel文件-用MultipartFile类读取excel文件以后用transferTo()转存到硬盘上(这里待处理）
	public int addManySt(MultipartFile file) {
		if(!file.isEmpty()){
			//这里有bug待调--windows上便没有这个问题
            File excelfile = new File("D:\\学员管理系统\\upload\\"+ file.getOriginalFilename());
            try {
                file.transferTo(excelfile);
                List<ExcelRow> rows=ExcelUtil.getExcel("D:\\学员管理系统\\upload\\"+ file.getOriginalFilename(), "sheet1", 1);
                List<Student> students=new ArrayList<Student>();
                for(ExcelRow row:rows) {
                	SimpleDateFormat sdf=new SimpleDateFormat("yyyy-MM-dd");
                	if(row.cells.size()>4) {
                		java.util.Date date=sdf.parse(row.cells.get(4));
                    	students.add(new Student(row.cells.get(0),row.cells.get(2),row.cells.get(3),date));
                	}
                }
                return mapper.batchInsert(students);
            }catch (Exception e) {
                excelfile.delete();
                e.printStackTrace();
            }
        }else{
            return 0;
        }
		return 1;
	}
//mapper层
@InsertProvider(type = MyProvider.class, method = "batchInsert")
    public int batchInsert(List<Student> students);
	class MyProvider {
		/* 批量插入 */
        public String batchInsert(Map map) {
        	List<Student> students = (List<Student>) map.get("list");
            StringBuilder sb = new StringBuilder();
            sb.append("INSERT ALL  ");
            for(int i=0;i<students.size();i++) {
            	SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd");
            	String dateString = formatter.format(students.get(i).getStbirthday());
            	sb.append(" INTO T_STUDENTS (stname,stidnumber,stphone,stbirthday,stisdelete) VALUES ");
            	sb.append(" ('"+students.get(i).getStname()+"','"+students.get(i).getStidnumber()+"','"+students.get(i).getStphone()+"',TO_DATE('"+dateString+""+"', 'yyyy-mm-dd'),'0') ");
            }
           sb.append(" SELECT 1 FROM DUAL ");
            return sb.toString();
        }
	}
```

---

- 查询数据、显示数据

---

前端部分将输入框、按钮放在表单元素中，利用**表单提交**来调用后端，最后通过**数据表格**来显示数据。
```
//表单
<form class="layui-form tmsform">
  <div class="layui-form-item">
    <div class="layui-inline">
      <label class="layui-form-label">关键字:</label>
      <div class="layui-input-inline">
        <input name="keyword" autocomplete="off" class="layui-input">
      </div>
      <div class="layui-input-inline">
        <button type="submit" class="layui-btn" lay-submit="" lay-filter="demo1">查询</button>
      </div>
    </div>
    <button type="button" class="layui-btn layui-btn-radius" id="import">导入Excel</button>
  </div>
  </form>
//数据表格
<table id="demo" lay-filter="test"></table>
//js部分
var form=layui.form;
    var table = layui.table;
    form.on('submit(demo1)', function(data){
		var submitData=data.field;
		if(submitData.keyword!=''){
			ins1=table.render({
			    elem:'#demo'
			    ,url:'getAllStu'
			    ,cellMinWidth: 80 //全局定义常规单元格的最小宽度，layui 2.2.1 新增
			    ,cols: [
			    	[
			    	{field: 'stnumber', title: '序号'},
			    	{field: 'stname', title: '姓名'},
			    	{field: 'stidnumber', title: '身份证号码',width:180,},
			    	{field: 'stgender', title: '性别'},
			    	{field: 'stage', title: '年龄'},
			    	{field: 'clcount', title: '报班数量'},
			    	{field: 'stphone', title: '联系电话'},
			    	{fixed: 'right', title: '操作', toolbar: '#tableBtn', width:400,align: 'center'},
			    ]
			    	]
				,page:true
				,parseData: function(res)
			    {
					var data=res.message.data;
					//修改返回值的序号、年龄、性别属性
					for(let i=0;i<data.length;i++){
						data[i].stnumber=i+1;
						data[i].stage=GetAge(data[i].stbirthday);
						if(data[i].stgender==0)data[i].stgender='女';
						else data[i].stgender='男';
					}
					tabledata=data;
			    	//res 即为原始返回的数据
			        return {
			          "code": res.state, //解析接口状态
			          "msg": res.error, //解析提示文本
			          "count":res.message.data.length, //解析数据长度
			          "data": res.message.data //解析数据列表
			        };
			      }
			  });
		}else{
		}
		return false;
	  });
```

---

筛选查询的实现在sql中使用like关键字。**分页**操作、**按生日倒序**还未实践过！
