---
layout: post
title:  "Java Notes"
date:   2019-11-04
categories: Java

---

> Java Notes

---
- java的substring(begin,end)||substring(start)
包含开始索引，不包含结束索引。||包含开始索引，到结束。
- StringBuilder是变长的string，只需toString()方法就可以变成普通的string。
 StringBuilder怎么添加元素呢？
 append()
 StringBuilder怎么反转呢？
 reverse()
- hashmap优雅的初始化方式
```
        相信很多人和笔者一样，经常会做一些数组的初始化工作，
        也肯定会经常用到集合类。假如我现在要初始化一个String类型的数组，
        可以很方便的使用如下代码：　　
    String [] strs = {"Tom","Jack"};
    但是我相信很多人在初始化HashMap的时候是使用如下的方式：　　
    Map<String, Object> map = new HashMap<String, Object>();
　　map.put("name", "June");  
　　map.put("age", 12);
　　上面这段代码个人觉得略显啰嗦，其实还有另外一种优雅的初始化方式：　　
  Map<String, Object> map = new HashMap<String, Object>() {
    　　{
        　　put("name", "June");  
       　　 put("age", 12);  
    　　}
　　};
　　这边有必要说清楚两个大括号表示的是啥意思，是一种什么语法呢？其实，
外层的一组“{}”表示的是一个匿名类，内层的一对“{}”表示的是**实例初始化块**，
  然后这边还有一点需要明白，实例初始化块的代码在编译器编译过后，是放在类的构造函数里面的，并且是在原构造函数代码的前面。
```
  **实例初始化块**的解释：<u>https://www.cnblogs.com/jankuo/archive/2018/10/14/9787600.html</u>

- 二分法中求mid的防止溢出的操作为
mid=start+((end-start)>>1);注意后面要加上一个括号，因为优先级比较低的问题。
- hashmap怎么判断某个key是否存在？
map.containsKey();
- vector<int>怎么使用？
看错了，这个是C++里面的。Java中对应的是List<Integer>
- Java的string的start with怎么用？
startsWith()
- stack<String> 怎么变为string
遍历栈，使用string builder连接每一项，在用tostring()转为string
- 如何去掉string的最后一个字符
没有现成的函数，只能使用substring(0,str.length()-1)
- string转date
```

              String str = "2007-1-18";
              DateFormat format1 = new SimpleDateFormat("yyyy-MM-dd");
              Date date=null;
             try {   
                  date = format1.parse(str);  
             } catch (ParseException e) {   
                  e.printStackTrace();   
             }
```
- java的Integer参数是什么意思
int与integer的区别从大的方面来说就是基本数据类型与其包装类的区别：
int 是基本类型，直接存数值，而integer是对象，用一个引用指向这个对象
- 1.Java 中的数据类型分为基本数据类型和复杂数据类型
int 是前者而integer 是后者（也就是一个类）；因此在类进行初始化时int类的变量初始为0.而Integer的变量则初始化为null.
- 2.初始化时：
　　int i =1；Integer i= new Integer(1);(要把integer 当做一个类看)；但由于有了自动装箱和拆箱　　
　　使得对Integer类也可使用：Integer i= 1；　　　　
　　int 是基本数据类型（面向过程留下的痕迹，不过是对java的有益补充），Integer 是一个类，是int的扩展，定义了很多的转换方法
　　类似的还有：float Float;double Double;string String等，而且还提供了处理 int 类型时非常有用的其他一些常量和方法
　　举个例子：当需要往ArrayList，HashMap中放东西时，像int，double这种内建类型是放不进去的，因为容器都是装object的，这是就需要这些内建类型的外覆类了。
　　Java中每种内建类型都有相应的外覆类。
　　Java中int和Integer关系是比较微妙的。关系如下：
　　1.int是基本的数据类型；
　　2.Integer是int的封装类；
　　3.int和Integer都可以表示某一个数值；
　　4.int和Integer不能够互用，因为他们两种不同的数据类型；
　　举例说明
private void test(Integer iAge){
int age=iAge;
}
test(null);//将会导致空指针异常
　　并且泛型定义时也不支持int: 如：List<Integer> list = new ArrayList<Integer>();可以 而List<int> list = new ArrayList<int>();则不行
总而言之：如果我们定义一个int类型的数，只是用来进行一些加减乘除的运算or作为参数进行传递，那么就可以直接声明为int基本数据类型，但如果要像
对象一样来进行处理，那么就要用Integer来声明一个对象，因为java是面向对象的语言，因此当声明为对象时能够提供很多对象间转换的方式，与一些常用
的方法。自认为java作为一们面向对象的语言，我们在声明一个变量时最好声明为对象格式，这样更有利于你对面向对象的理解。
- queue入列和出列
offer/add：入列
区别：两者都是往队列尾部插入元素，不同的时候，当超出队列界限的时候，而offer（）方法是直接返回false，add（）方法是抛出异常让你处理。
poll/remove：出列
poll() 方法和 remove() 方法的区别？
poll() 和 remove() 都是从队列中取出一个元素，但是 poll() 在获取元素失败的时候会返回空，但是 remove() 失败的时候会抛出异常。
判断队列是否为空：isEmpty()
获取队列大小：size()
```
        //注意，queue中也可以插入了null，依旧输出null。就像其他的插入元素一样
        Queue<TreeNode> queue=new LinkedList<TreeNode>();
        queue.add(null);
        queue.add(root);
        System.out.println(queue.poll());
        System.out.println(queue.poll().val);
```
- java的异或
符号为：^
- list翻转
List<String> list = new ArrayList<>();
Collections.reverse(list);
list的大小：size()
list获取某一项：get()
list初始化
list变为string：
```
String.join("",pre)
```
- Java中的pair对
```
  import javafx.util.Pair;
 Pair<String, String> pair = new Pair<>("aku", "female"); pair.getKey(); pair.getValue();
 ```
 - java截取数组
System.arraycopy(源数组名称，源数组开始点，目标数组名称，目标数组开始点，拷贝长度);
- map遍历
```
Iterator<Map.Entry<Integer, Integer>> entries = map.entrySet().iterator();
        while(entries.hasNext()){
            Map.Entry<Integer, Integer> entry = entries.next();
            if(entry.getValue()==1)return entry.getKey();
        }
```
- java的vector
Vector 可实现自动增长的对象数组。 java.util.vector提供了向量类(Vector)以实现类似动态数组的功能。 创建了一个向量类的对象后，可以往其中随意插入不同类的对象，即不需顾及类型也不需预先选定向量的容量，并可以方便地进行查找。
遍历方法：
<u>https://blog.csdn.net/CentForever/article/details/47311891</u>
- java的string和date
string变为date
SimpleDateFormat sdf=new SimpleDateFormat("yyyy-MM-dd");
String dstr="2008-4-24";
java.util.Date date=sdf.parse(dstr);
- Java获取当天时间
Date date = new Date();//当前日期
SimpleDateFormat dateFormat= new SimpleDateFormat("yyyy-MM-dd :hh:mm:ss");
- java判断一个字符串是否包含某个字符
注意双引号
String str = "abc";
boolean status = str.contains("a");
- Java——字符串排序
注意：只能对于列表排序
List<String> list = new ArrayList<String>();
list.add("c");
list.add("b");
list.add("d");
list.add("a");
System.out.println(list);//[c, b, d, a]
Collections.sort(list);//对list集合进行排序
System.out.println(list);//[a, b, c, d]
- string不能使用for each
将string变成字符数组，即使用toCharArray()函数
- Java的栈变成string
String.valueOf(stack);
- java的int的最大值
java int 的最大值 Integer.MAX_VALUE
- java判断完全平方数
其实就是从整数value/2开始，判断该数的平方是不是本身，一直到0就停止。
(注意要取整）：因为要是取double的话，有的时候虽然平方等于value，但是value却不是完全平方数。
```
public static boolean isSquares2(int value) {
        if (value < 0) {
            return false;
        }

        int item = value / 2;
        for (int index = item; index >= 0; index--) {
            if (index * index == value) {
                return true;
            }
        }
        return false;

    }
```
- java的布尔数组的初值
都是false
- java的double转变为string
直接加一个“”最简单
```
Double toBeString = 400.40;
String fromDouble = "" + toBeString;
```
- java获取键值+Java将set变成数组+Set转数组
其实就是将键值集合set变成数组
```
Integer[] res=map.keySet().toArray(new Integer[0]);

```
- java将hashmap按照value排序
将entrySet转换为List,然后重写比较器比较即可.这里可以使用List.sort(comparator),也可以使用Collections.sort(list,comparator)  
转换为list
```
 List<Map.Entry<String, Integer>> list = new ArrayList<Map.Entry<String, Integer>>(phone.entrySet()); //转换为list
```
使用list.sort()排序--按照value生序哦
```
list.sort(new Comparator<Map.Entry<String, Integer>>() {
          @Override
          public int compare(Map.Entry<String, Integer> o1, Map.Entry<String, Integer> o2) {
              return o1.getValue().compareTo(o2.getValue());
          }
      });
```
- sc.nextLine
next()从遇到第一个有效字符（非空格、换行符）开始扫描，遇到第一个分隔符或结束符（空格’ ‘或者换行符 ‘\n’）时结束。 nextLine()则是扫描剩下的所有字符串知道遇到回车为止。**
总之就是获取string的话使用next()
- java的约分
关键在于求最大公约数
```
public static void main(String[] args) {
		int a = 7, b = 100, gongyinshu = 1;
		int smaller = a > b ? b : a;
		for (int i = 1; i <= smaller; i++) {
			if (a % i == 0 && b % i == 0) {
				gongyinshu = i;
			}
		}
		System.out.println(a + "和" + b + "的最小公因数为：" + gongyinshu);
		System.out.println(a + "/" + b + "约分得：" + a / gongyinshu + "/" + b / gongyinshu);
}
```
- java的math.random
产生一个[0，1)之间的随机数
int number = (int)(Math.random()*4+1)；取值正好是[1,5)
- java判断某个字串
```
String str1 = "abcdefg";
int result1 = str1.indexOf("ab");
或者
contains("4")
```
- Java字符串翻转
StringBuffer(str).reverse().toString();
- break跳出哪层循环
break只能跳出一层循环
- java的string以什么开头
```
String str = “file_123464574322.jpg”;  
boolean start= str.startsWith(“file_”);
```
- java的long变成string
```
可以用String类的valueOfString sR=String.valueOf(longVal)可以用Long类的toStringString sR=Long.toString(longVal)
```
- java停止控制台输入  
```
输入eof即可
```
- java局部变量没有赋值，能使用吗  
```
不能。但是Java的成员变量没有赋值可以使用，在类的对象初始化的时候，会被默认赋值。
```



  
