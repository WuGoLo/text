---
title: ArcGis学习笔记
date: 2019-11-26 00:05:31
tags: ArcGis
---

一、ArcGis是什么？ArcGis有些什么软件，分别是干什么的？

ArcGis有些什么软件
>ArcGIS Desktop 9.3 是桌面地理信息系统分析工具，用来做各做空间分析。
>ArcGIS Engine 9.3 是组件GIS，可以用来开发一个地理信息系统，需要安装VS2005或2008。
>ArcGIS Server 9.3 和Engine差不多，用来开发大型GIS项目。
>ArcGIS SDE 9.3 建立数据库的空间数据库引擎。
>ArcGIS IMS 9.3 用来建立WEBGIS服务！！
>其中ArcGIS Desktop 9.3
>ArcGIS Engine 9.3
>ArcGIS Server 9.3
>都隶属于AO对象！

ArcGis基础底图及引用天地图和各类库创建

~~~js
import * as esriLoader from 'esri-loader'
import { resolve } from 'path';
export default {
  initMap(mapdom) {
    esriLoader.loadCss(); // 引入css版本
    const options = {
      url: 'https://js.arcgis.com/4.7/'
      // css: true
    }
    esriLoader.loadModules([
      'esri/Map',
      'esri/views/MapView',
      'esri/layers/support/TileInfo',
      'esri/layers/WebTileLayer',
      'esri/layers/Layer',
      "esri/config",
      'esri/layers/ElevationLayer',
      "dojo/domReady!"
    ],options)
    .then(([
      Map, 
      MapView,
      TileInfo,
      WebTileLayer,
      Layer,
      esriConfig,
      ElevationLayer
    ]) => {
      // let esriAPI = {

      // }
      esriConfig.request.corsEnabledServers.push("localhost:6080");//设置地图服务器已允许跨域
      // 我们是通过瓦片形式加载天地图的
      // 天地图根据投影又分为两种：墨卡托和经纬度
      // 经纬度投影的情况下比较复杂，且需要注意的地方比较多，加载过程如下

      // 首先我们设定瓦片信息，天地图经纬度地图的切片信息全部使用该信息设定
      var tileInfo = new TileInfo({
        dpi: 90.71428571427429,
        rows : 256,
        cols : 256,
        compressionQuality : 0,
        origin : {
          x : -180,
          y : 90
        },
        spatialReference : {
          wkid : 4326
        },
        lods : [
          {level : 2,levelValue: 2, resolution : 0.3515625, scale : 147748796.52937502},
          {level : 3,levelValue: 3, resolution : 0.17578125, scale : 73874398.264687508},
          {level : 4,levelValue: 4,resolution : 0.087890625, scale : 36937199.132343754},
          {level : 5,levelValue: 5, resolution : 0.0439453125, scale : 18468599.566171877},
          {level : 6,levelValue: 6, resolution : 0.02197265625, scale : 9234299.7830859385},
          {level : 7,levelValue: 7, resolution : 0.010986328125, scale : 4617149.8915429693},
          {level : 8,levelValue: 8, resolution : 0.0054931640625, scale : 2308574.9457714846},
          {level : 9,levelValue: 9, resolution : 0.00274658203125, scale : 1154287.4728857423},
          {level : 10,levelValue: 10, resolution : 0.001373291015625, scale : 577143.73644287116},
          {level : 11,levelValue: 11, resolution : 0.0006866455078125, scale : 288571.86822143558},
          {level : 12,levelValue: 12, resolution : 0.00034332275390625, scale : 144285.93411071779},
          {level : 13,levelValue: 13, resolution : 0.000171661376953125, scale : 72142.967055358895},
          {level : 14,levelValue: 14, resolution : 8.58306884765625e-005, scale : 36071.483527679447},
          {level : 15,levelValue: 15, resolution : 4.291534423828125e-005, scale : 18035.741763839724},
          {level : 16,levelValue: 16, resolution : 2.1457672119140625e-005, scale : 9017.8708819198619},
          {level : 17,levelValue: 17, resolution : 1.0728836059570313e-005, scale : 4508.9354409599309},
          {level : 18,levelValue: 18, resolution : 5.3644180297851563e-006, scale : 2254.4677204799655},
          { level: 19,levelValue: 19, resolution: 2.68220901489257815e-006, scale: 1127.23386023998275 },
          { level: 20,levelValue: 2, resolution: 1.341104507446289075e-006, scale: 563.616930119991375 }
        ]
      })

      // 根据尝试得到如下经验：
      // 当WebTiledLayer 初始化时设置了 tileInfo 属性时，模板字段必须去掉 $ 也就是 {……} 而不是 ${……}
      // 同时不要相信文档中的替换说明

      // 在加载经纬度地图的时候我们需要使用 {subDomain}, {col}, {row}, {level}分别替换服务器列表，瓦片列编号，瓦片行编号，当前缩放(显示)级别
      // 经纬度矢量地图瓦片的URL:
      // http://t4.tianditu.com/DataServer?T=vec_c&x=27&y=3&l=5

      // 分析上述 URL 我们知道，域名中的 t4 部分代表子域字段，参数列表中的TILECOL, TILEROW, TILEMATRIX 分别对应列编号， 行编号，缩放(显示)级别， 对这几个部分进行替换，得到 url 模板如下
      // http://{subDomain}.tianditu.com/DataServer?T=vec_c&x={col}&y={row}&l={level}
      // 经过查询资料天地图瓦片可用子域分别有 t0,t1,t2,t3,t4,t5,t6,t7 八个子域
      // 根据现有信息新建 WebTiledLayer 如下

      var layer = WebTileLayer('http://{subDomain}.tianditu.com/DataServer?T=vec_c&x={col}&y={row}&l={level}&tk=174705aebfe31b79b3587279e211cb9a',{
        // subDomains: ['t0','t1','t2','t3','t4','t5','t6','t7'],
        subDomains: ['t0'],
        tileInfo: tileInfo
      })
      var layer_anno = WebTileLayer('http://{subDomain}.tianditu.com/DataServer?T=cva_c&x={col}&y={row}&l={level}&tk=174705aebfe31b79b3587279e211cb9a',{
        //subDomains: ['t0','t1','t2','t3','t4','t5','t6','t7'],
        subDomains: ['t0'],
        tileInfo: tileInfo
      })

      // 加载server地图

      // 创建地图，不设置底图，如果设置底图会造成坐标系无法被转换成 ESPG:4326 (WGS1984)
      var map = new Map({
        spatialReference : {
          wkid : 4326
        },
        basemap: {
          baseLayers: [layer, layer_anno]
        }
      });
      var elevLyr = new ElevationLayer({
        // Custom elevation service
        url: "http://localhost:6080/arcgis/rest/services/beixinjichu/北信基础/MapServer"
      });
      // Add elevation layer to the map's ground.
      map.ground.layers.add(elevLyr);
      // 创建MapView
      var view = new MapView({
        container: mapdom,
        spatialReference : {
          wkid : 4326
        },
        map: map,
        center: [116.46, 39.92],
        //1:scale的图
        scale: 2000
      });
    }).catch (err => { console.log(err) })
  }
}
~~~

