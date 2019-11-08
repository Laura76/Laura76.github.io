---
layout: post
title:  "WebGL Notes"
date:   2019-11-07
categories: WebGL Arcgis

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
