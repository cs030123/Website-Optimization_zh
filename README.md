## 网站性能优化项目

### 项目介绍

本项目用一个简单的网站展示了简单的个人及课程信息，点击首页上的链接，可查看课程的详情。页尾还提供了pizza预定功能。

### 项目运行方式

双击index.html，即可打开首页。点击首页中的链接，可转向相关的课程详情，也可通过最后的“Cam's Pizzeria”预定pizza。

_** 若要使网站能正常展示运行，请确保网络能正常连通以下地址：_
1. Google字体库 https://fonts.googleapis.com/css?family=Open+Sans:400,700
2. http://www.google-analytics.com/analytics.js
3. https://lh4.ggpht.com
4. https://developers.google.com

### 代码优化明细

* index.html
1. 压缩图片profilepic.jpg、所有CSS和js文件
2. 延迟加载字体库，挪到window的onload事件中加载
3. 2个js文件的加载添加async标志
4. print.css添加媒体标志print

* views/js/main.js
1. determineDx仅用于返回当前slider设置对应的百分比，遍历pizza改变宽度的方法changePizzaSizes将计算新宽度的过程提取到for循环外
``` bash
// 返回slider当前代表的pizza百分比尺寸，由changePizzaSlices(size)函数调用   
// {string} size，1:0.25, 2:0.3333, 3:0.5  
function determineDx (size) {      
      switch(size) {        
      case "1":         
        return 0.25;        
      case "2":          
        return 0.3333;        
      case "3":          
        return 0.5;        
      default:          
      console.log("bug in sizeSwitcher");      
      }  
 }
 
 // 根据当前slider的设置，遍历披萨的元素并改变它们的宽度  
 function changePizzaSizes(size) {    
      var randomWidth = document.querySelector("#randomPizzas").offsetWidth;    
      var newwidth = (determineDx(size) * randomWidth) + 'px';    
      var pizzas = document.querySelectorAll(".randomPizzaContainer");    
      for (var i = 0; i < pizzas.length; i++) {      
        pizzas[i].style.width = newwidth;    
      }  
  }
```
2. 基于滚动条位置移动背景中的披萨滑窗的方法updatePositions将计算scrollTop提取到for循环外
``` bash
  var scrollTop =  window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
  var items = document.querySelectorAll('.mover');  
  for (var i = 0; i < items.length; i++) {    
    var phase = Math.sin((scrollTop / 1250) + (i % 5));    
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';  
  }
```
