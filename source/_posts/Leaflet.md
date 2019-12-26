---
title: Leaflet
date: 2019-12-13 15:32:09
tags: 超图
---

# 超图Leaflet

学习Leaflet，首先要明白必须几点，这是前提：

1. Leaflet需要超图桌面软件idesktop 9D及以上版本来发布服务iserver(iserver版本9D及以上)；
2. 超图展示地图用的是地理坐标（-180，180）而一般idesktop和iserver做出来并发布的都是像素坐标（-120000000类似的数据）。**所以后端服务给的数据，是要先转换才能用的**；
3. 投影CRS是什么？简单来说就是地球是个球，桌面是个平面。就是将拿到的坐标值投影到桌面上的一个规定(投影的规定不止一种，参考[CRS](https://leafletjs.com/reference-1.6.0.html#crs))
4. 地图和矢量几何图层是两码事，地图用的是地理坐标，矢量图层是像素【展示操作必须互转】
5. 理解像ps一样的图层渲染地图方式；
6. 以上几条不是废话！重点！重点！重点！下边开始上代码详细解释以上几点

一些方便学习GIS开发的网站：

- GIS开发者：https://www.giserdqy.com/gis/leaflet/6465
- 麻辣GIS：<https://malagis.com/> 

## 一、初始化地图

案例：

初始准备大概介绍

- idesktop绘好图，**之后转坐标系**，通用转ESGP：3857坐标系
- iserver浏览器地址登陆本地服务端，默认地址：<http://localhost:8090/iserver> 
- 其他发布地图的方式可以去官网看，发布的服务类型也很多，[Leaflet官网](http://iclient.supermap.io/web/apis/leaflet.html)

初始化地图代码：

~~~js
# 引入Leaflet包
import L from 'leaflet';
import '@supermap/iclient-leaflet';
export default {
  # 初始化地图函数
  init(id) { // id:地图容器DOM元素的id
    // let host = window.isLocal ? window.server : "http://support.supermap.com.cn:8090";
    // let url = host + "/iserver/services/map-china400/rest/maps/China";
  	// 这里地址可以用官方事例地址，下边是局域网地址
    let url = "http://172.19.1.183:8090/iserver/services/map-bxdt/rest/maps/bxdt";
  
    //leaflet CRS投影设置，iserver发布的服务地图坐标系是3857
    var scaleDenominators = [40000, 20000, 10000, 5000, 2500, 1250]; 
    var crs = L.Proj.CRS("EPSG:3857", {
      origin: [40.310,116.750], // 地图原点
      scaleDenominators: scaleDenominators // 比例尺必须设置
    }); // 圆点，缩放比例尺寸可到iserver上查看

		# 初始类L.map
    const map = L.map(id, {
      crs: crs,
      center: [40.310, 116.750],
      maxZoom: 5,
      zoom: 0
    })
    L.supermap.tiledMapLayer(url).addTo(map);
  }, // 地图初始化完成
}
~~~

![1576224562825](Leaflet\1576224663634.png)

> 此图的参数数字都是像素点，并不是坐标点

### 1、添加marker标记中心点

~~~js
# 需引入图标icon，leaflet资源文件的路径
import icon from "leaflet/dist/images/marker-icon.png";
import iconShadow from "leaflet/dist/images/marker-shadow.png";
// 定义icon
let DefaultIcon = L.icon({
  iconUrl: icon,
  shadowUrl: iconShadow
});
L.Marker.prototype.options.icon = DefaultIcon;
// 使用icon，创建一个marker标记点
L.marker([40.310,116.750]).addTo(map);
~~~

### 2、数据与地图坐标点互转

~~~js
# 官方CRS和point，latlng的几个方法，首先要去官方了解这三个是什么？
var point = L.point(12949090.25 , 4855990.22);
var point_new2 = L.CRS.EPSG3857.unproject(point);
console.log(point_new2) // 结果： LatLng {lat: 39.931480, lng: 116.323656}
var lawej = L.latLng(point_new2.lat, point_new2.lng)
var point_new1 = L.CRS.EPSG3857.project(lawej);
console.log(point_new1) // 结果： Point {x: 12949090.25, y: 4855990.219999999}
~~~

> 刚开始啥都不懂，初始化地图。拿到服务地址，坐标系不对转了一周才搞定。之后把像素点当坐标点了，也研究了老半天┓(;´_｀)┏，关键网速很烂，地图很大加载浏览器加载也慢。
>
> 建议初学无人指点，可先用superMap Clissic，官方文档很好理解，也没转像素点这个麻烦事。

## 二、Leaflet图层解析

### 开发前必备知识：

> Leaflet的所有API类都是层层继承关系，子类layer大多继承父类的method，event，attribute

Leaflet 1.0.0 简化的UML 类图。Leaflet拥有60多个JavaScript类，因此UML图有点大，使用L.ImageOverlay来制作可缩放的图片。

可缩放图片地址：http://leafletjs.com/examples/extending/class-diagram.html

图层方法、类、事件继承关系：

![img](Leaflet\891477-20171026152403633-352354328.png) 

## 三、矢量图层渲染

笔记中例子均以vue项目中应用为例

官方链接： [Vector Layer](https://leafletjs.com/reference-1.6.0.html#path)

应用： 主要用来渲染地图中的几何图形，点、线、面、路径等。是覆盖在基础底图上的一些图层

- **path**: 下边各个几何图形的基类，不会直接来用。本身继承自Layer
- **polyline** ： 创建线对象，继承自path
- **polygon**： 创建多边形对象，继承自polyline
- **rectangle**： 创建矩形对象，继承自polygon
- **circle**： 创建圆对象，继承自circleMarker
- **circleMarker** ： 可以用于创建标记点（更多功能看官网），继承自path
- SVGOverlay、SVG、canvas ： 这三个用的少，不做解释，看官网

继承关系图：

![1576565775984](E:\学习文档\Myblog\text\source\_posts\Leaflet\1576565775984.png)

### **L.circleMarker**

创建标记点

栗子：

create_point.js

~~~javascript
/* 
	自己封装的一个创建标记点的js文件
	引入npm下载的leaflet包
*/
import L from 'leaflet';
import '@supermap/iclient-leaflet';
// 设置圆点样式，具体配置可到path类中看options配置
const STYLE = {
  radius: 4,
  color: '#ffb400',
  weight: 2,
  fill: true,
  fillColor: '#ffb400',
  fillOpacity: 1
}
export default {
  Creat(map,data) { // data是具体点坐标值，例如：[45.51, -122.68]
    var point = L.circleMarker(data, STYLE);
    return point
  }
}
~~~

> 问题解答： 出现latlng 和point等的错误提示，则是**坐标数据**未转化为**坐标对象**，转化看开篇坐标转换的讲解

### L.polyline

创建线

简单栗子：

~~~javascript
// 通过三个点创建一条红色的线
var latlngs = [
    [45.51, -122.68],
    [37.77, -122.43],
    [34.04, -118.2]
];
// 配置线样式
var options = {
  color: 'red',
  width: 3
}
var polyline = L.polyline(latlngs, options).addTo(map);
// 让地图中心视口移动到当前线图层的范围，可以不写这步
map.fitBounds(polyline.getBounds());
~~~

项目栗子：

create_polyline.js

~~~javascript
import L from 'leaflet';
import '@supermap/iclient-leaflet';
export default {
  defaultStyle: {
    color: '#10ac10',
    weight: 5
  },
  overStyle: {
    color: 'red',
    weight: 7
  },
  Creat(map, data, info) {
    let datas = this.formatData(data)
    let arr = []; // 保存输出的一条条线图层
    datas.forEach((item,index) => { // 拿到数据进行遍历，数据是坐标点二维数组
      if(datas[index+1]) {
        let latlngs = [
          item,
          datas[index+1],
        ];
        let polyline = L.polyline(latlngs, this.defaultStyle);
        polyline.info = info // 这里可以添加
        // 添加各种线点击的事件。e.target即为此条线
        polyline.on('click', (e) => {
          console.log(e.target)
        })
        polyline.on('mouseover', (e) => {
          e.target.setStyle({ color: 'red' })
        }) // setStyle是线继承自path类的方法，更多方法参看官方path
        polyline.on('mouseout', (e) => {
          e.target.setStyle(this.defaultStyle)
        })
        arr.push(polyline)
      }
    })
    // 将一条管道路径绘制到一个要素图层组里
    let feature = L.featureGroup(arr) // L.featureGroup创建要素图层组
    .on('click', (e) => { map.fitBounds(e.target.getBounds()) })
    return feature
  },
	// 转换数据坐标系，我们服务给的是EPSG：3857转换后的像素点，需要转换为leaflet需要的坐标点
  formatData(data) {
    let pointArr = [];
    data.forEach((item, index) => {
      let point = L.point(item[0] , item[1]);
      let newpoint = L.CRS.EPSG3857.unproject(point); // 此方法初始化地图也有讲解
      pointArr.push(newpoint)
    })
    return pointArr
  }
}
~~~

### L.polygon

创建多边形

简单例子：

~~~javascript
// 通过坐标二维数组创建
var style =  {
  color: 'red',
  width: 10,
}
var latlngs = [[37, -109.05],[41, -109.03],[41, -102.05],[37, -102.04]];
var polygon = L.polygon(latlngs, ).addTo(map);
// zoom the map to the polygon
map.fitBounds(polygon.getBounds());
~~~

> 之后的几个图形创建，官方例子也比较详细。项目中应用可以参考L.polyline中的例子，都是类似的方法。

### 矢量图层组

#### L.layerGroup 

#### L.featureGroup

这俩类，也是类似ps中多图层编组，方便对多个图层统一管理。例如：所有创建的线图层可以放到一个featureGroup中。

具体用法请看官方文档：<https://leafletjs.com/reference-1.6.0.html#layergroup> 

例子：

~~~javascript
L.layerGroup([marker1, marker2])
    .addLayer(polyline)
    .addTo(map);
// featureGroup继承自layerGroup，但是又有扩展的一些方法。如：getBounds()
// getBounds()，只有featureGroup中有，图层事件中运用起来很爽，谁用谁知道。
L.featureGroup([marker1, marker2, polyline])
    .bindPopup('Hello world!')
    .on('click', function() { alert('Clicked on a member of the group!'); })
    .addTo(map);
~~~

