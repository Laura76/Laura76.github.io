---
layout: post
title:  "WebGL Notes"
date:   2019-11-07
categories: WebGL Arcgis

---

> WebGL 实践记录

---

- 开放视频和坐标点初始化接口  
目的：未来可能会有着**数量众多视频**的需求，通过开放接口，以方便**添加**和**删除**视频，从而提高网页的速度。  
<u>旧的固定视频初始化的流程:</u>  
在**setup函数**（external renderer添加到图层后执行的方法，只执行一次）中，setupVideo创建新的video元素和load读取obj文件。  
在**render**（重复性执行）函数中，每一个group即面执行animateTexture函数，传入的参数是该面对应的视频。   
<u>新的开放参数的流程：</u>  
new ExternalRenderer();  
createTex(objX,videoX);  
deleteTex(objX,videoX);  
大致的思路是新建两个数组变量videos和objs，用来存储多次绘画过程中的视频和obj文件。  
当createTex的时候，首先要完成视频和obj文件的初始化工作（分别是setupVideo方法和load方法），再就将参数中的obj和video存储至之前的两个数组变量。  
当deleteTex的时候，同创建视频的时候类似，第一步也是要完成视频的删除收尾工作，这里我做的主要就是将视频变量执行pause(),src="",load()三个方法，最后再将两个数组变量中指定的元素的delete属性置为true。  
在render即绘画的方法中，要注意判断该obj文件的delete元素是否是true，是的话该obj对应的物体应该跳过。（obj中有很多的group，group是每次draw方法的执行单元即三角形单元)。
