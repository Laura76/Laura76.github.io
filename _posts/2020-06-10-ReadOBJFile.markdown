---
layout: post
title:  "ReadOBJFile"
date:   2020-06-10
categories: study  up

---

> 读取OBJ文件


---
- 简单手绘图解  
![手绘图解](../pics/微信图片_20200610161704.jpg)
- 解析过程简述  
首先new一个loadObjs对象。  
loadObjs是一个函数，这个函数的有一个属性this.objects，this.objects是一个数组。  
loadObjs的原型的方法有load函数，函数的参数是一个数组，数组内是很多obj文件的路径。该
方法中调用**loadOBJ**方法，返回值被存储到this.objects数组中,返回值就是文件读取的结果。  
- loadOBJ方法    
该方法先new一个**OBJModel**对象。再new一个XMLHttpRequest请求，使用该请求对象来打开obj文件
。这个请求时异步的，当文件加载完毕即request.readyState==4的时候，再开始读文件即**onReadOBJFile**方法。
- OBJModel对象  
是一个函数，有很多属性:vertices/groups数组等
- onReadOBJFile方法  
参数会传入某个**OBJModel**对象和XMLHttpRequest请求的响应内容即obj文件的内容即request.responseText。使用这些参数来调用这个**OBJModel**对象的readOBJFile方法。  
- readOBJFile方法-OBJModel对象的原型方法  
参数是fileString即请求的响应内容  
首先使用换行符将这个fileString字符串分行，成为一个lines数组。之后遍历该数组。数组遍历结束之后，将OBJModel对象的**complete**属性设置为true。  
数组遍历的内容：首先在遍历之前，new一个StringParser字符串解析对象sp。之后，可以执行循环遍历了。
首先是sp.init(line)即初始化每一行，再得到sp.getWord();  
接下来开始解析获取的单词了：如果是#则跳过是注释，如果是mtllib则是读取材质文件暂不解析。如果是v则是
读取顶点，vn是法向，vt是纹理，如果是g则是组对象，usemtl是当前组对象使用的材质名称，f是面信息。  
v读取顶点：首先使用getWord方法获取下一个单词，再将此单词转为float数，依次类推获取点的xyz坐标。将这三个
坐标创建vec3对象，将vec3存到OBJModel对象的vertices数组属性中。  
g组对象:如果前一组对象没有顶点，则将其从group数组中弹出。接下来，每次都new一个Group对象，并将该对象推到
OBJModel对象的groups数组属性中。  
f面信息:调用this.parseFace方法，传入sp和当前的Group对象。
- parseFace方法-OBJModel对象的原型方法 
解析该行，并将解析出的顶点的索引存到变量vIndices数组中，如果vIndices的长度超过了3则要拆分成多个三角形。
这个拆分顺序即三角形的顺序我就不记录了。将vIndices数组里的所有元素都添加到Group对象的vertices属性中。
- StringParser对象  
是一个函数，属性有this.str(待解析的字符串)/this.index(字符串中处理到的位置),和一个方法init。  
init方法：将传入的str参数赋值给属性this.str,将this.index赋值为0。  
getWord方法：首先是调用自己的this.skipDelimiters()来清除掉字符串前面的分隔符像空格括号之类的，
再调用getWordLength()方法（遇到分隔符会停止）来获取字符串中一个单词的长度，字符串中可能会有很多单词。
再将单词返回，别忘了把this.index更新为这个单词的后面。


