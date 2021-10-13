---
title: Maven Helper的使用
index_img:  /img/21.jpg
banner_img: /img/21.jpg
categories: java
tags: [笔记]
date:  2021-04-20 16:30:42
---

## [](#前言 "前言")前言
  boot项目报jar包冲突了,但是我是有在父pom工程中申明了依赖版本的.后来发现是申明的依赖版本比子工程中关联的依赖版本低了的问题，在父工程中申明高版本的依赖就好了。。。。
  过程中也用到了之前没有用到的mavenHelper,感觉挺有用的，记录一下
## [](#什么是ar包冲突 "什么是ar包冲突")什么是ar包冲突
首先理解一下为什么会有jar包冲突,先了解依赖的传递
{% note warning%}
- **依赖传递**
    <br/>
    <br/>
当我们需要A的依赖的时候，就会在pom.xml中引入A的jar包；而引入的A的jar包中可能又依赖B的jar包，这样Maven在解析pom.xml的时候，会依次将A、B 的jar包全部都引入进来。

 {% endnote %} 

  那么 jar包冲突是怎么产生的呢?
 {% note warning%}
- **jar包冲突原理**
    <br/>
    <br/>
 假设有如下依赖关系：  
    <br/>
    <br/>
   A->B->C->D1(log 15.0)：A中包含对B的依赖，B中包含对C的依赖，C中包含对D1的依赖，假设是D1是日志jar包，version为15.0
   <br/>
   <br/>
   E->F->D2(log 16.0)：E中包含对F的依赖，F包含对D2的依赖，假设是D2是同一个日志jar包，version为16.0
  {% endnote %} 
  
  当pom.xml文件中引入A、E两个依赖后，根据Maven传递依赖的原则，D1、D2都会被引入，而D1、D2是同一个依赖D的不同版本。
  当我们在调用D2中的method1()方法，而D1中是15.0版本（method1可能是D升级后增加的方法），可能没有这个方法，这样JVM在加载A中D1依赖的时候，找不到method1方法，就会报NoSuchMethodError的错误，此时就产生了jar包冲突。

## [](#解决办法 "解决办法")解决办法

  之前我都是用的比较笨的办法，自己靠感觉来找，不好找的情况下就使用依赖关联图来看,就是pom文件里面右键-Diagrams-Show Dependencies。
  但是这种方式用起来并不太友善,比如这样：
    
  ![1](http://123.57.9.108/2021/04/20/Maven-Helper的使用mage/1.jpg)
  
  很乱,而且很多时候报红报冲突的也不止一个，很难准确定位到冲突的jar包。
      <br/>
      <br/>
  
  所以这次解决问题的时候找到了一个比较好用的插件，Maven-Helper。idea中-settings-pulgins-  
    ![2](http://123.57.9.108/2021/04/20/Maven-Helper的使用mage/2.jpg)
    
  安装插件后，打开项目pom.xml文件，点击：dependcy Analyzer,界面如下：  
  ![3](http://123.57.9.108/2021/04/20/Maven-Helper的使用mage/3.png)

  点击common-cli,如下：  
   ![4](http://123.57.9.108/2021/04/20/Maven-Helper的使用mage/4.png)
     
   可以看出依赖包有1.2和1.3.1的包，存在包冲突，排查1.3.1的包即可，排除方法如下：选中右侧1.3.1的版本：右键，点击jump to left tree ,如下：  
   ![5](http://123.57.9.108/2021/04/20/Maven-Helper的使用mage/5.png)
     
   右键点击exclude，一键排除冲突包，点击reimport,刷新依赖即可，可以看到common-cli不存在冲突列表：  
      ![6](http://123.57.9.108/2021/04/20/Maven-Helper的使用mage/6.png)