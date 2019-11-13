> ArcGis地图一同官方文档
>
> [官方地址 》](https://developers.arcgis.com/javascript/latest/sample-code/intro-widgets/index.html)

# 一、基础使用

## 》MapView简介（2D）

本教程将引导您逐步了解如何在2D MapView中创建简单地图。

### 1.引用ArcGIS API for JavaScript

首先，设置一个类似于以下示例的基本HTML文档：

```jsp
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no" />
    <title>Intro to MapView - Create a 2D map</title>
  </head>
</html>
```

在`<head>`标记内，使用`<script>`和`<link>`标记引用ArcGIS API for JavaScript ：

```jsp
<link rel="stylesheet" href="https://js.arcgis.com/4.13/esri/themes/light/main.css" />
<script src="https://js.arcgis.com/4.13/"></script>
```

该`<script>`标签加载从CDN通过ArcGIS API为JavaScript。发行新版本的API后，请更新版本号以匹配新发行的版本。

该`<link>`标记引用`main.css`样式表，其中包含特定于Esri小部件和组件的样式。

### 2.加载模块

使用第二个`<script>`标签从API加载特定的模块。使用以下代码段中的语法加载以下模块：

- `esri/Map` -加载特定于创建地图的代码
- `esri/views/MapView` -加载允许以2D模式查看地图的代码

```jsp
<script>
  require([ "esri/Map", "esri/views/MapView" ], function(Map, MapView) {
    // Code to create the map and view will go here
  });
</script>
```

在构建简单页面或进行实验时，将JavaScript放在script标签中很有用，但不适用于大型应用程序。在构建更大的应用程序时，所有JavaScript都应位于单独的.js文件中。

全局require（）函数（由Dojo提供）用于加载模块。Esri的JavaScript API构建在Dojo之上，以利用Dojo的模块化代码库以及协调跨浏览器差异的能力。有关Dojo和JavaScript API之间的关系的更多信息，请参阅[使用Dojo](https://developers.arcgis.com/javascript/jshelp/inside_dojo.html)和[Why Dojo](https://developers.arcgis.com/javascript/jshelp/why_dojo.html)。

### 3.创建地图

使用新建一个地图`Map`，该地图是对从`esri/Map`模块加载的Map类的引用。您可以`basemap`通过将一个对象传递给Map构造函数来指定地图属性，例如。

```js
require(["esri/Map", "esri/views/MapView"], function(Map, MapView) {
  var map = new Map({
    basemap: "streets"
  });
});
```

其他底图的选项有：`satellite`，`hybrid`，`topo`，`gray`，`dark-gray`，`oceans`，`osm`，`national-geographic`。通过修改[沙箱中](https://developers.arcgis.com/javascript/latest/sample-code/sandbox/index.html?sample=intro-mapview)的`basemap`选项来使用备用底图。查看[Map类](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)以获取有关其他地图选项的更多详细信息。

### 4.创建2D视图

查看用作HTML文件中的容器的参考节点，从而允许用户查看HTML页面内的地图。`MapView`通过将对象传递给构造函数来创建一个新的并设置其属性：

```
require(["esri/Map", "esri/views/MapView"], function(Map, MapView) {
  var map = new Map({
    basemap: "streets"
  });

  var view = new MapView({
    container: "viewDiv", // Reference to the DOM node that will contain the view
    map: map // References the map object created in step 3
  });
});
```

在此代码段中，我们将`container`属性设置为将保存地图的DOM节点的名称。该`map`属性引用我们在上一步中创建的地图对象。请参阅[MapView文档](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)以获取您可以在视图上设置的其他属性，包括`center`和`zoom`，这些属性可以用于定义视图的初始范围。

有两种类型的视图：MapView（用于查看2D地图）和SceneView（用于查看3D地图）。[单击此处](https://developers.arcgis.com/javascript/latest/sample-code/intro-sceneview/index.html)以了解有关创建具有3D视图的地图的更多信息。

### 5.定义页面内容

现在，创建地图和视图所需的JavaScript已完成！下一步是添加关联的HTML以查看地图。对于此示例，HTML非常简单：添加一个`<body>`标签（用于定义在浏览器中可见的内容）以及将`<div>`在内部创建视图的单个元素。

```
<body>
  <div id="viewDiv"></div>
</body>
```

在`<div>`有“viewDiv”的ID匹配传递到的MapView的标识`container`在constructor属性。

### 6.设置页面样式

使用中的`<style>`标签设置页面内容的样式`<head>`。要使地图填充浏览器窗口，请在页面的内添加以下CSS `<style>`：

```
<style>
  html,
  body,
  #viewDiv {
    padding: 0;
    margin: 0;
    height: 100%;
    width: 100%;
  }
</style>
```

现在，您已经使用ArcGIS JavaScript for JavaScript 4.0构建了第一个2D Web应用程序！您的最终HTML代码应如下所示：

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no" />
    <title>Intro to MapView - Create a 2D map</title>
    <style>
      html,
      body,
      #viewDiv {
        padding: 0;
        margin: 0;
        height: 100%;
        width: 100%;
      }
    </style>
    <link rel="stylesheet" href="https://js.arcgis.com/4.13/esri/themes/light/main.css" />
    <script src="https://js.arcgis.com/4.13/"></script>
    <script>
      require(["esri/Map", "esri/views/MapView"], function(Map, MapView) {
        var map = new Map({
          basemap: "streets"
        });
        var view = new MapView({
          container: "viewDiv", // Reference to the scene div created in step 5
          map: map, // Reference to the map object created before the scene
          zoom: 4, // Sets zoom level based on level of detail (LOD)
          center: [15, 65] // Sets center point of view using longitude,latitude
        });
      });
    </script>
  </head>
  <body>
    <div id="viewDiv"></div>
  </body>
</html>
```



## 》图层简介

图层是地图的最基本组成部分。它是代表实际现象的图形或图像形式的空间数据的集合。图层可能包含存储矢量数据的离散要素，也可能包含存储栅格数据的连续像元/像素。

地图可能包含不同类型的图层。有关API中可用的每种图层类型的广泛概述，请参见“图层”的类说明中的[此表](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#layer-types)。

所有图层都从[Layer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html)继承属性，方法和事件。本教程将讨论其中的一些常用属性。要了解特定于不同图层类型的属性，请[在样本中搜索该](https://developers.arcgis.com/javascript/latest/sample-code/intro-layers/index.html?search=*layer)图层类型（例如[SceneLayer](https://developers.arcgis.com/javascript/latest/sample-code/intro-layers/index.html?search=scenelayer)）。

在完成以下步骤之前，您应该熟悉[views](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html)和[Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)。如有必要，请首先完成以下教程：

- [MapView简介](https://developers.arcgis.com/javascript/latest/sample-code/intro-mapview/index.html)
- [SceneView简介](https://developers.arcgis.com/javascript/latest/sample-code/intro-sceneview/index.html)

### 1.创建一个Map，一个SceneView和一个复选框输入HTML元素。

创建一个基本[Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)并将其添加到[SceneView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)实例。您的JavaScript可能类似于以下代码：

```js
require(["esri/Map", "esri/views/SceneView"], function(Map, SceneView) {
  // Create the Map
  var map = new Map({
    basemap: "oceans"
  });

  // Create the SceneView
  var view = new SceneView({
    container: "viewDiv",
    map: map
  });
});
```

在HTML主体中添加一个复选框元素。该元素的目的将在本教程的后面部分讨论。

```js
<body>
  <div id="viewDiv"></div>
  <span id="layerToggle" class="esri-widget"> <input type="checkbox" id="streetsLayer" checked /> Transportation </span>
</body>
```

### 2.使用TileLayer创建两个图层

在为创建地图和视图而编写的代码之前，创建两个[TileLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-TileLayer.html)实例。为此，您必须需要`esri/layers/TileLayer`模块并在层上指定[url](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-TileLayer.html#url)属性。该`url`属性必须指向托管在ArcGIS Server或Portal for ArcGIS上的缓存地图服务。

用于访问ArcGIS服务的所有图层均具有`url`属性，必须将其设置为在视图中呈现要素。在此示例中，我们将使用[Esri世界运输服务](https://server.arcgisonline.com/ArcGIS/rest/services/Reference/World_Transportation/MapServer)以及包含有关纽约市住房密度数据的服务为街道和高速公路创建图层。

```js
require([
  "esri/Map",
  "esri/views/SceneView",
  "esri/layers/TileLayer" // Require the TileLayer module
], function(Map, SceneView, TileLayer) {
  var transportationLayer = new TileLayer({
    url: "https://server.arcgisonline.com/ArcGIS/rest/services/Reference/World_Transportation/MapServer"
  });

  var housingLayer = new TileLayer({
    url: "https://tiles.arcgis.com/tiles/nGt4QxSblgDfeJn9/arcgis/rest/services/New_York_Housing_Density/MapServer"
  });

  /*****************************************************************
   * The code to create a map and view instance in the previous step
   * should be placed here.
   *****************************************************************/
});
```

### 3.在图层上设置其他属性

您可以在图层上设置其他属性，包括[id](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#id)，[minScale](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#minScale)，[maxScale](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#maxScale)，[opacity](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#opacity)和[visible](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#visible)。这些可以在构造函数中设置，也可以在应用程序中另一点的实例上直接设置。

我们将`id`在每个图层上添加一个，并在运输图层上设置不透明度。

```js
var transportationLayer = new TileLayer({
  url: "https://server.arcgisonline.com/ArcGIS/rest/services/Reference/World_Transportation/MapServer",
  id: "streets",
  opacity: 0.7
});

var housingLayer = new TileLayer({
  url: "https://tiles.arcgis.com/tiles/nGt4QxSblgDfeJn9/arcgis/rest/services/New_York_Housing_Density/MapServer",
  id: "ny-housing"
});
```

的`id`唯一标识层，因此很容易在应用程序的其它部分参考。如果开发人员没有直接设置，则在创建图层时会自动生成。

的`minScale`和`maxScale`不同尺度属性控制层的可见性。使用这些属性可以在一定比例下改善应用程序性能，并增强地图的制图性能。该`visible`属性是`true`默认设置。

### 4.将图层添加到地图

可以通过几种方式将图层添加到地图。这些都在[Map.layers](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#layers)的文档中进行了[讨论](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#layers)。在此示例中，我们将以不同的方式将每个图层添加到地图中。

将外壳层添加到地图的构造函数中。

```js
// Both layers were created prior to this code snippet
var map = new Map({
  basemap: "oceans",
  layers: [housingLayer] // layers can be added as an array to the map's constructor
});
```

使用[map.layers.add（）](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#add)将运输层添加到地图上。

```js
map.layers.add(transportationLayer);
```

现在，这两个图层都应该在视图中可见。

### 5.处理图层的可见性

使用该`addEventListener`方法侦听第一步中创建的复选框元素上的更改事件。选中或禁用此框后，将打开或关闭运输层的可见性。像一样`visible`，可以直接在图层实例上设置任何[图层的](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html)任何属性。这在下面的代码段中完成。

```js
require(["esri/Map", "esri/views/SceneView", "esri/layers/TileLayer"], function(
  Map,
  SceneView,
  TileLayer
) {
  /*****************************************************************
   * All code previously written in the steps above should be placed
   * before the following code
   *******************************************************************/

  // Create a variable referencing the checkbox node
  var streetsLayerToggle = document.getElementById("streetsLayer");

  // Listen to the change event for the checkbox
  streetsLayerToggle.addEventListener("change", function() {
    // When the checkbox is checked (true), set the layer's visibility to true
    transportationLayer.visible = streetsLayerToggle.checked;
  });
});
```

即使此示例中的视图对图层不可见，该图层仍作为地图的一部分存在。因此，即使用户可能看不到视图中渲染的图层，您仍可以访问该图层的所有属性并将其用于分析。

### 6.了解LayerViews

该[层](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html)对象管理作为服务发布的地理数据和表格数据。它不处理在[View中](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html)渲染图层。那就是[LayerView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-LayerView.html)的工作。图层的LayerView在视图中呈现之前创建。使用[FeatureLayers时](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)，相应的[FeatureLayerView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-FeatureLayerView.html)可以为开发人员提供对与该图层的[要素](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-FeatureLayerView.html)有关的视图中渲染的图形的访问权限。

在此步骤中，我们将侦听视图的[layerview-create](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#event-layerview-create)事件，并打印房屋和运输层的LayerViews，以便您可以在控制台中浏览它们的属性。请注意，我们将使用`id`在步骤3中为每个图层创建的来获取所需的图层。除了地图的操作图层外，还会针对底图图层和高程图层触发此事件。

```js
require(["esri/Map", "esri/views/SceneView", "esri/layers/TileLayer"], function(
  Map,
  SceneView,
  TileLayer
) {
  /*****************************************************************
   * All code previously written in the steps above should be placed
   * before the following code
   *******************************************************************/

  // This event fires each time a layer's LayerView is created for the
  // specified view instance
  view.on("layerview-create", function(event) {
    if (event.layer.id === "ny-housing") {
      // Explore the properties of the housing layer's layer view here
      console.log("LayerView for New York housing density created!", event.layerView);
    }
    if (event.layer.id === "streets") {
      // Explore the properties of the transportation layer's layer view here
      console.log("LayerView for streets created!", event.layerView);
    }
  });
});
```

### 7.使用Layer.when（）

层是一个[承诺](https://developers.arcgis.com/javascript/latest/guide/programming-patterns/#promises)，可在[加载](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#loaded)时或在其所有属性对开发人员可用时解决。在此示例中，我们希望将视图[动画](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#goTo)[化为](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#fullExtent)外壳层的[fullExtent](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#fullExtent)，因为我们可能不知道用于初始化视图的良好范围（或居中和缩放）。

在`fullExtent`加载之前，我们无法获取图层。因此，一旦诺言解决，我们就必须处理动画。这是通过[when（）](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#when)处理的。

```js
// When the layer's promise resolves, animate the view to the layer's fullExtent
housingLayer.when(function() {
  view.goTo(housingLayer.fullExtent);
});
```

### 8.总结

还有更多的图层属性未在此处讨论。要了解有关与特定图层有关的其他属性的更多信息，请参阅此页目录中列出的[API文档](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html)和示例。可以在[沙箱中](https://developers.arcgis.com/javascript/latest/sample-code/sandbox/index.html?sample=intro-layers)查看本教程的最终代码。



## 》弹出窗口简介

![1573629243370](ArcGis官方文档中文\1573629243370.png)

[弹出窗口](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html)通过显示信息以响应用户操作，提供了一种向ArcGIS JavaScript应用程序添加交互性的简便方法。每个[视图](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html)都有一个与之关联的[弹出窗口](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#popup)。在大多数情况下，弹出窗口的[内容](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#content)允许用户从[图层](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html)和[图形](https://developers.arcgis.com/javascript/latest/api-reference/esri-Graphic.html)访问数据属性。

虽然弹出窗口通常与图形层或要素层一起使用，但是您也可以显示弹出窗口以响应查询或不涉及图形或要素的某些其他操作。例如，您可以在视图中显示用户单击位置的纬度/经度坐标。

此示例将引导您如何使用默认[弹出](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html)一个[视图](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html)通过设置其属性，如`content`，`title`和`location`，没有从拉动信息显示它[PopupTemplate](https://developers.arcgis.com/javascript/latest/api-reference/esri-PopupTemplate.html)，[图形](https://developers.arcgis.com/javascript/latest/api-reference/esri-Graphic.html)或[层的功能](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#features)。该示例使用“ [定位器”任务](https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-Locator.html)对视图上单击位置的某个点进行反向地理编码。返回的地址显示在弹出窗口的[内容中，](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#content)而所单击位置的纬度和经度显示在弹出窗口的[标题内](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#title)。

在完成以下步骤之前，您应该熟悉[views](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html)和[Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)。如有必要，请首先完成以下教程：

- [MapView简介](https://developers.arcgis.com/javascript/latest/sample-code/intro-mapview/index.html)
- [SceneView简介](https://developers.arcgis.com/javascript/latest/sample-code/intro-sceneview/index.html)

### 1.需要Locator，Map和MapView模块并创建新实例

使用[世界地理编码服务](https://geocode.arcgis.com/arcgis/index.html)创建[定位器](https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-Locator.html)。创建一个基本[地图](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)。将[Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)实例添加到[View](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html)。您的JavaScript看起来应类似于以下代码：

```js
require(["esri/tasks/Locator", "esri/Map", "esri/views/MapView"], function(Locator, Map, MapView) {
  // Create a locator task using the world geocoding service
  var locatorTask = new Locator({
    url: "https://geocode.arcgis.com/arcgis/rest/services/World/GeocodeServer"
  });

  // Create the Map
  var map = new Map({
    basemap: "streets-navigation-vector"
  });

  // Create the MapView
  var view = new MapView({
    container: "viewDiv",
    map: map,
    center: [-71.6899, 43.7598],
    zoom: 12
  });
});
```

### 2.收听视图的单击事件，并在单击的位置显示弹出窗口

在视图上侦听[click事件](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#event-click)，并获取单击位置的经度和纬度。在单击的位置显示弹出窗口，并在弹出的[标题中](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#title)显示单击的位置的坐标。为此，我们将在[open（）](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#open)方法中设置弹出窗口的[位置](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#location)和[标题](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#title)属性。

```js
view.popup.autoOpenEnabled = false;
view.on("click", function(event) {
  // Get the coordinates of the click on the view
  // around the decimals to 3 decimals
  var lat = Math.round(event.mapPoint.latitude * 1000) / 1000;
  var lon = Math.round(event.mapPoint.longitude * 1000) / 1000;

  view.popup.open({
    // Set the popup's title to the coordinates of the clicked location
    title: "Reverse geocode: [" + lon + ", " + lat + "]",
    location: event.mapPoint // Set the location of the popup to the clicked location
  });
});
```

### 3.找到单击位置的地址，然后在弹出的内容中显示匹配的地址

单击的位置用作反向地理编码方法的输入，结果地址显示在弹出窗口的[内容中](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#content)。用户单击视图后，调用[locatorTask.locationToAddress（）](https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-Locator.html#locationToAddress)方法并传递被单击的点以获取该位置的地址。如果找到该点的匹配地址，则设置弹出窗口的`content`属性以显示该地址。最后，如果找不到所单击位置的地址，则弹出窗口将`content`显示一条通用消息，指示未找到地址。

```js
var params = {
  location: event.mapPoint
};

// Execute a reverse geocode using the clicked location
locatorTask
  .locationToAddress(params)
  .then(function(response) {
    // If an address is successfully found, show it in the popup's content
    view.popup.content = response.address;
  })
  .catch(function(error) {
    // If the promise fails and no result is found, show a generic message
    view.popup.content = "No address was found for this location";
  });
```


## 》小部件简介

![1573629650334](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573629650334.png)

使用小部件是ArcGIS API for JavaScript的重要组成部分。通常，将小部件视为封装了一组特定功能的API的一部分。该API提供了具有预定义功能的即用型小部件，其中一些功能包括：

- 通过“ [定位”](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Locate.html)小部件在地图上定位当前位置，
- 添加图例以使用“ [图例”](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Legend.html)小部件帮助可视化地图，
- 显示一个图层列表，为用户提供一种使用[LayerList](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-LayerList.html)小部件打开/关闭它们的简便方法，
- 或使用“ [搜索”](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Search.html)小部件在地图上[搜索](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Search.html)要素和位置。

有关所有可用小部件的完整列表，请参阅[API参考中](https://developers.arcgis.com/javascript/latest/api-reference/#modules-in-esri-widgets)的`esri/widgets`文件夹。

默认情况下，某些窗口小部件会自动提供给[MapView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)和[SceneView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)，而其他窗口小部件则需要手动添加到应用程序中。所述[缩放](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Zoom.html)小部件是一个自动添加作为视图的一部分的一个例子。尽管默认情况下添加了“缩放”小部件，但可能无法自动打开所有小部件。例如，“缩放”小部件提供了相当标准的功能，并且在整个应用程序中使用，而与应用程序的主要用途无关。鉴于，除非特别需要在应用程序中具有地理位置功能，否则让视图自动添加诸如“ [定位”之](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Locate.html)类的小部件是没有意义的。

综上所述，无论使用什么小部件，使用小部件的基本概念在各个方面都是一致的。这些概念是：

1. 创建并实例化小部件
2. 设置小部件的属性

本教程将向您展示如何向[MapView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)添加[BasemapToggle](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html)小部件。此小部件可让您在单个[View中](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html)在两个[底图](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#basemap)之间切换。切换后的底图设置在[视图的](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html#view)[地图](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#map)对象内。本教程将涉及上述三个要点。

在完成以下步骤之前，您应该熟悉[views](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html)和[Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)。如有必要，请首先完成以下教程：

- [MapView简介](https://developers.arcgis.com/javascript/latest/sample-code/intro-mapview/index.html)
- [SceneView简介](https://developers.arcgis.com/javascript/latest/sample-code/intro-sceneview/index.html)

尽管本教程描述了如何在2D [MapView中](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)使用BasemapToggle小部件，但同样的前提也适用于3D [SceneView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)。

有关完全控制窗口小部件样式的信息，请参见[样式](https://developers.arcgis.com/javascript/latest/guide/styling/index.html)主题。

### 1.创建一个简单的Map并在MapView中进行设置

首先创建一个简单的[Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)并在[View中](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)引用它。您的JavaScript可能类似于以下代码：

```js
require(["esri/Map", "esri/views/MapView", "esri/widgets/BasemapToggle"], function(
  Map,
  MapView,
  BasemapToggle
) {
  // Create the Map with an initial basemap
  var map = new Map({
    basemap: "topo"
  });

  // Create the MapView and reference the Map in the instance
  var view = new MapView({
    container: "viewDiv",
    map: map,
    center: [-86.049, 38.485],
    zoom: 3
  });
});
```

### 2.创建BasemapToggle小部件并设置其属性

接下来，创建[BasemapToggle](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html)小部件的实例并设置其[view](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html#view)和[nextBasemap](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html#nextBasemap)属性。

的[视图](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html#view)属性可以是[MapView类](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)或[SceneView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)。它提供了对初始[底图](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#basemap)的BasemapToggle小部件访问权限，您可以在其中通过视图的[map](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#map)属性进行切换。

该[nextBasemap](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html#nextBasemap)属性设置[底图](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#basemap)用于切换。此属性接受以下任一项：

- 任何Esri底图的[字符串ID](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#basemap)。例如，一些可能的值是：“街道”，“混合”，“拓扑”等。
- 自定义[底图](https://developers.arcgis.com/javascript/latest/api-reference/esri-Basemap.html)对象。例如，假设您要在自己的服务器（或第三方服务器）上发布了自定义切片服务。您也可以在此处指定。

就本教程而言，我们将指定一个字符串ID `hybrid`。设置属性将完成小部件的创建。

您的JavaScript代码应类似于以下内容：

```js
var view = new MapView({
  container: "viewDiv",
  map: map,
  center: [-86.049, 38.485],
  zoom: 3
});

// 1 - Create the widget
var toggle = new BasemapToggle({
  // 2 - Set properties
  view: view, // view that provides access to the map's 'topo' basemap
  nextBasemap: "hybrid" // allows for toggling to the 'hybrid' basemap
});
```

### 3.将小部件放置在视图中

最后，我们需要指定此小部件在[View的UI](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#ui)属性中的放置位置。此属性显示视图中可用的默认窗口小部件，并允许您打开和关闭它们。除此之外，您可以指定要将这些小部件放置在何处。该[UI](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#ui)财产继承关的[DefaultUI](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ui-DefaultUI.html)。此类提供了可以在其中添加，移动或删除窗口小部件的方法。在这种情况下，我们将BasemapToggle小部件添加到视图的右上角。

您的JavaScript代码应类似于以下内容：

```js
// Add widget to the top right corner of the view
view.ui.add(toggle, "top-right");
```

现在，您有一个简单的地图，显示了如何使用BasemapToggle小部件。我们演示了小部件创建的两个基本部分：实例化和设置属性。除了在View中停靠和放置。

您的最终JavaScript代码应如下所示：

```js
require(["esri/Map", "esri/views/MapView", "esri/widgets/BasemapToggle"], function(
  Map,
  MapView,
  BasemapToggle
) {
  // Create the Map with an initial basemap
  var map = new Map({
    basemap: "topo"
  });

  // Create the MapView and reference the Map in the instance
  var view = new MapView({
    container: "viewDiv",
    map: map,
    center: [-86.049, 38.485],
    zoom: 3
  });

  // 1 - Create the widget
  var toggle = new BasemapToggle({
    // 2 - Set properties
    view: view, // view that provides access to the map's 'topo' basemap
    nextBasemap: "hybrid" // allows for toggling to the 'hybrid' basemap
  });

  // Add widget to the top right corner of the view
  view.ui.add(toggle, "top-right");
});
```

# 二、映射和视图

## 1、加载基本的网络地图

![1573630428484](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573630428484.png)

该地图使用一个[WebMap](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebMap.html)，该[WebMap](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebMap.html)按州显示意外死亡。标记的大小指示每种状态下的意外死亡总数，而颜色指示发生率。鲜红色表示每10万人中更多的意外死亡，而深绿色表示每10万人中更少的意外死亡。该[WebMap](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebMap.html)是从ArcGIS Online的或门户项目的ArcGIS访问到自定义Web应用程序。

所需的只是门户中WebMap项的项ID。

创建一个新的[WebMap](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebMap.html)实例，并在WebMap 的[portalItem](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebMap.html#portalItem)属性内设置门户项目ID 。由于WebMap扩展了[esri / Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)，因此您可以`map`在[MapView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)的属性上设置网络地图实例。

```js
/************************************************************
 * Creates a new WebMap instance. A WebMap must reference
 * a PortalItem ID that represents a WebMap saved to
 * arcgis.com or an on-premise portal.
 ************************************************************/

var webmap = new WebMap({
  portalItem: {
    id: "e691172598f04ea8881cd2a4adaa45ba"
  }
});

/************************************************************
 * Set the WebMap instance to the map property in a MapView.
 ************************************************************/
var view = new MapView({
  map: webmap,
  container: "viewDiv"
});
```

要引用本地门户中的项目，请在[esriConfig.portalUrl中](https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#portalUrl)设置门户的URL 。

请参阅“ [使用ArcGIS Platform”](https://developers.arcgis.com/javascript/latest/guide/working-with-platform/index.html)以获取有关ArcGIS API for JavaScript如何利用门户项目的信息。

## 2、加载基本的Web场景

同1图

该示例演示如何将[Web场景](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)从ArcGIS for Portal项目加载到自定义应用程序中。该场景说明了冬至（2014年6月21日）上新西兰奥塔哥皇后镇周围群山的阴影。每条线代表一天中特定时间周围群山投射的阴影的边缘。

加载Web场景很容易。所需的只是门户中WebScene项的项ID。

创建一个新的[WebScene](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)实例，并在[WebScene的portalItem](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html#portalItem)属性内设置门户项目ID 。由于WebScene扩展了[esri / Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)，因此您可以`map`在[SceneView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)的属性上设置Web场景实例。

```
/************************************************************
 * Creates a new WebScene instance. A WebScene must reference
 * a PortalItem ID that represents a WebScene saved to
 * arcgis.com or an on-premise portal.
 ************************************************************/

var scene = new WebScene({
  portalItem: {
    id: "3a9976baef9240ab8645ee25c7e9c096"
  }
});

/************************************************************
 * Set the WebScene instance to the map property in a SceneView.
 ************************************************************/
var view = new SceneView({
  map: scene,
  container: "viewDiv"
});
```

要引用本地门户中的项目，请在[esriConfig.portalUrl中](https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#portalUrl)设置门户的URL 。

请参阅“ [使用ArcGIS Platform”](https://developers.arcgis.com/javascript/latest/guide/working-with-platform/index.html)以获取有关ArcGIS API for JavaScript如何利用门户项目的信息。

## 3、保存网络场景

![1573630821145](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573630821145.png)

此示例演示如何通过创建新项或覆盖[从ArcGIS for Portal项加载](https://developers.arcgis.com/javascript/latest/sample-code/webscene-basic/index.html)的现有项将[WebScene](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)保存到[ArcGIS for Portal项](https://developers.arcgis.com/javascript/latest/sample-code/webscene-basic/index.html)。

保存Web场景很容易。只需一个[WebScene](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)和一个有效的[门户网站](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-Portal.html)即可将场景保存到该[门户网站](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-Portal.html)。

### 保存到新项目

创建一个新的空[WebScene](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)实例和一个应将WebScene保存到其中的[Portal](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-Portal.html)实例。加载门户网站将触发用户身份验证，如果成功，则该项目将保存到给定的门户网站。

```
/************************************************************
 * Creates a new empty WebScene instance. An empty WebScene
 * must have at least the basemap property defined to be
 * functional.
 ************************************************************/
var scene = new WebScene({
  basemap: "topo"
});

/************************************************************
 * Create a new Portal instance and request immediate user
 * authentication.
 ************************************************************/
var portal = new Portal({
  authMode: "immediate"
});

/************************************************************
 * Loading the portal will trigger authentication and once the
 * returned Promise is resolved, the WebScene will be saved as
 * a new PortalItem using the given title.
 ************************************************************/
portal.load().then(function() {
  scene.saveAs({
    title: "Empty WebScene",
    portal: portal
  });
});
```

### 覆盖现有项目

创建一个新的[WebScene](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)实例，并在[WebScene的portalItem](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html#portalItem)属性内设置门户项目ID 。加载门户网站将触发用户身份验证，如果成功，则该项目将保存到给定的门户网站。

```
/************************************************************
 * Creates a new WebScene instance. A WebScene can reference
 * a PortalItem ID that represents a WebScene saved to
 * arcgis.com or an on-premise portal.
 ************************************************************/
var scene = new WebScene({
  portalItem: {
    id: "3a9976baef9240ab8645ee25c7e9c096"
  }
});

/************************************************************
 * Create a new Portal instance and request immediate user
 * authentication.
 ************************************************************/
var portal = new Portal({
  authMode: "immediate"
});

/************************************************************
 * Loading of either the scene or the portal will trigger
 * authentication and once both Promises are resolved, the
 * Webscene will be saved thereby overwriting the previously
 * loaded PortalItem with any changes that have occurred.
 ************************************************************/
scene.load().then(function() {
  portal.load().then(function() {
    scene.portalItem.title = "Modified WebScene";
    scene.portalItem.portal = portal;
    scene.save();
  });
});
```

要使用本地门户网站的加载或保存项目，请在[esriConfig.portalUrl中](https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#portalUrl)设置门户网站的URL 。

请参阅“ [使用ArcGIS Platform”](https://developers.arcgis.com/javascript/latest/guide/working-with-platform/index.html)以获取有关ArcGIS API for JavaScript如何利用门户项目的信息 

## 4、网络场景幻灯片

## 5、创建本地场景

![1573631007903](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573631007903.png)

此示例演示如何创建本地场景并将其添加到[SceneView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)。在此样本中，场景添加了两层-一层描述了堪萨斯州南部的油气井，另一层则显示了附近地震的位置。这些图层在场景的表面以及下方渲染。

![下面的本地场景](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\scene-local-subsurface.png)

您可以在底图下方导航，以查看地震相对于附近油气井的真实位置和深度的确切位置。要探索表面以下的特征，必须倾斜视图。要在表面下方倾斜，请右键单击视图，然后向上拖动鼠标。右键单击并向下拖动鼠标，使其在表面上方向后倾斜。[单击此处](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#navigation)以了解有关SceneView中导航的更多信息。

[SceneView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)的[viewingMode](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#viewingMode)属性确定场景是还是。设置[clipingArea](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#clippingArea)以定义局部场景的边界。`global``local`

该[navigationConstraint](https://developers.arcgis.com/javascript/latest/api-reference/esri-Ground.html#navigationConstraint)在地面属性控制用户的导航地面以下地方场面的能力。

```
// Creating a new map
var map = new Map({
  basemap: "topo"
});

// Add the scene to a SceneView
var view = new SceneView({
  container: "viewDiv",
  map: map,
  // Indicates to create a local scene
  viewingMode: "local",
  // Use the exent defined in clippingArea to define the bounds of the scene
  clippingArea: kansasExtent
});
```

## [6、地球相机3D动画goTo()](https://developers.arcgis.com/javascript/latest/sample-code/scene-goto/index.html)

![1573631246169](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573631246169.png)

此示例说明如何使用[goTo](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#goTo)方法的options属性来自定义相机飞行动画。

这是一个示例，说明如何定义选项以使一般缓慢的飞行在到达目标时速度变慢：

```
var options = {
  speedFactor: 0.1, // animation is 10 times slower than default
  easing: "out-quint" // easing function to slow down when reaching the target
};

view.goTo(target, options);
```

## 7、查看填充

![1573631432513](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573631432513.png)

此示例显示如何在[MapView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)上使用[padding](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#padding)属性。使用它可以使[center](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#center)，[range](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#extent)等在全视图的子部分上工作。设置填充后，您可以将UI元素放置在填充区域中，并仍然使地图在视图中正确居中。

在此示例中，宽度为320px的侧边栏位于视图右侧的顶部。我们可以将视图中心偏移相同的宽度，以便将缩放和UI元素正确放置在剩余的视图空间中。

```
// Same value as the #sidebar width in CSS
view.padding.right = 320;
```

## 8、使用门户网站地图

您可以使用[Portal](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-Portal.html)在ArcGIS API for JavaScript应用程序中创建[底图](https://developers.arcgis.com/javascript/latest/api-reference/esri-Basemap.html)。这样，您可以利用组织的[默认底图](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-Portal.html#defaultBasemap)或[默认矢量底图](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-Portal.html#defaultVectorBasemap)。您可以定义[portalUrl](https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html#portalUrl)在[esriConfig](https://developers.arcgis.com/javascript/latest/api-reference/esri-config.html)和[部件BasemapGallery](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapGallery.html)将使用配置的底图在该门户网站。

### 1.初始化门户网站实例

```
// Default is portal for arcgis.com
var portal = new Portal();
```

### 2.加载门户网站资源

```js
// If you define the Portal URL in esriConfig, the
// BasemapGallery widget can determine which basemaps
// to use.
esriConfig.portalUrl = "https://jsapi.maps.arcgis.com";
// Intialize a portal instance and load it
var portal = new Portal();
portal
  .load()
  .then(function() {
    // A portal can be configured to use Vector Basemaps
    // by default or not.
    var basemap = portal.useVectorBasemaps ?
          portal.defaultVectorBasemap : portal.defaultBasemap;
    var map = new Map({
      basemap: basemap
    });
    var view = new MapView({
      container: "viewDiv",
      map: map,
      center: [-118.24, 34.073],
      scale: 10000
    });
    // The BasemapGallery will use the basemaps
    // configured by the Portal URL defined in esriConfig.portalUrl
    var basemapGallery = new BasemapGallery({
      view: view
    });
    var bgExpand = new Expand({
      view: view,
      content: basemapGallery
    });
    view.ui.add(bgExpand, "top-right");
  })
  .catch(function(error) {
    console.warn(error);
  });
});
```

这使您可以使用组织设置在整个组织中创建一致的应用程序。

## 9、创建自定义底图

![1573631853849](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573631853849.png)

此示例演示如何创建自定义[底图](https://developers.arcgis.com/javascript/latest/api-reference/esri-Basemap.html)并将其添加到[SceneView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)的[BasemapToggle](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html)小部件中。底图是和图层的简单容器。`baseLayers`

一个[WebTileLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-WebTileLayer.html)从第三方缓存服务创建并添加到一个新底图的`baseLayers`属性，因此它可以被用作备用底图。

```js
// Create a WebTileLayer with a third-party cached service
var mapBaseLayer = new WebTileLayer({
  urlTemplate: "https://stamen-tiles-{subDomain}.a.ssl.fastly.net/terrain/{level}/{col}/{row}.png",
  subDomains: ["a", "b", "c", "d"],
  copyright:
    'Map tiles by <a href="http://stamen.com/">Stamen Design</a>, ' +
    'under <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a>. ' +
    'Data by <a href="http://openstreetmap.org/">OpenStreetMap</a>, ' +
    'under <a href="http://creativecommons.org/licenses/by-sa/3.0">CC BY SA</a>.'
});

// Create a Basemap with the WebTileLayer. The thumbnailUrl will be used for
// the image in the BasemapToggle widget.
var stamen = new Basemap({
  baseLayers: [mapBaseLayer],
  title: "Terrain",
  id: "terrain",
  thumbnailUrl: "https://stamen-tiles.a.ssl.fastly.net/terrain/10/177/409.png"
});

// Add the custom basemap to the map
var map = new Map({
  basemap: stamen,
  ground: "world-elevation"
});
```

## 10、在同一视图中切换地图

此示例演示如何从ArcGIS for Portal项目初始化多个[WebMap](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebMap.html)。然后，您可以在单个视图中交换活动Webmap。

此示例使用以下网络地图：

- [失踪移民](https://www.arcgis.com/home/item.html?id=ad5759bf407c4554b748356ebe1886e5) -显示国际移民组织（IOM）的所有失踪移民报告。亮蓝色符号表示2015年或以后提交的报告，深蓝色符号表示2015年之前的事件。较大的符号表示报告的死亡人数更多。
- [难民路线](https://www.arcgis.com/home/item.html?id=71ba2a96c368452bb73d54eadbd59faa) -显示进入欧洲的难民路线。
- [2015年欧洲海到达量](https://www.arcgis.com/home/item.html?id=45ded9b3e0e145139cc433b503a8f5ab) -显示2015年1月至2015年7月海上难民到达欧洲南部的情况。

首先，创建一个WebMap数组而不加载它们。

```js
var webmaps = webmapids.map(function(webmapid) {
  return new WebMap({
    portalItem: {
      id: webmapid
    }
  });
});
```

然后，根据操作（例如单击按钮），可以通过在视图对象中引用新的Web地图来交换地图。

```js
var webmap = webmaps[id];
view.map = webmap;
```

请参阅“ [使用ArcGIS Platform”](https://developers.arcgis.com/javascript/latest/guide/working-with-platform/index.html)以获取有关ArcGIS API for JavaScript如何利用门户项目的信息。

## 11、2D切换3D

该示例演示了如何将地图的视图从2D切换到3D。可以使用相同的[Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)实例或单独的[WebMap](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebMap.html)和[WebScene](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)实例来完成。使用WebMaps和WebScenes可以减少检查两个视图都不支持的2D和3D之间差异的代码量，例如2D和3D符号体系。单击`3D`视图左上角的按钮，将视图从2D切换到3D，反之亦然。

要实现这一行为，将[容器](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#container)中的观点出发到目的地视图的容器。同样，在目标视图上设置[地图](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#map)实例和[视点](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#viewpoint)。

虽然[相机](https://developers.arcgis.com/javascript/latest/api-reference/esri-Camera.html)是用于设置一个优选的对象[SceneView的](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)可见程度和[程度](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#extent)优选为[MapView类](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)，所述[视点](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#viewpoint)上的每个视图属性是方便的，因为它仍然存在的[程度](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#extent)和[旋转](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#rotation)从一个视图类型到下一个。在2D MapView中，某些属性（例如[倾斜](https://developers.arcgis.com/javascript/latest/api-reference/esri-Camera.html#tilt)和[高程图层](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#ground)）将被忽略。

下面的功能演示了如何完成此操作。

```
// Switches the view from 2D to 3D and vice versa
function switchView() {
  var is3D = appConfig.activeView.type === "3d";

  // remove the reference to the container for the previous view
  appConfig.activeView.container = null;

  if (is3D) {
    // if the input view is a SceneView, set the viewpoint on the
    // mapView instance. Set the container on the mapView and flag
    // it as the active view
    appConfig.mapView.viewpoint = appConfig.activeView.viewpoint.clone();
    appConfig.mapView.container = appConfig.container;
    appConfig.activeView = appConfig.mapView;
    switchButton.value = "3D";
  } else {
    appConfig.sceneView.viewpoint = appConfig.activeView.viewpoint.clone();
    appConfig.sceneView.container = appConfig.container;
    appConfig.activeView = appConfig.sceneView;
    switchButton.value = "2D";
  }
}
```

### 关于在2D和3D之间切换的限制

请记住，从MapView切换到SceneView需要仔细考虑许多因素。请参阅以下列表以了解一些要考虑的因素。此列表并不全面。

- **数据类型** -由于3D的性质，某些图层不支持2D。这些包括[SceneLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-SceneLayer.html)，[IntegratedMeshLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-IntegratedMeshLayer.html)和[PointCloudLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-PointCloudLayer.html)。它们将需要完全从地图实例中删除，或替换为2D副本。例如，您可以将表示建筑物的SceneLayer与表示建筑物轮廓的多边形[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)切换。[ElevationLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-ElevationLayer.html)不需要与上述其他图层紧密联系，因为MapViews中忽略了[Map的地面](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#ground)。
- **符号** -虽然大多数[2D符号](https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Symbol.html)可在SceneView中使用，但MapViews中不支持所有[3D符号](https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Symbol3D.html)。如果使用3D符号，则可能需要重新配置[渲染器](https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html)。
- **UI组件** -如果使用[view.ui.add（）](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-ui-DefaultUI.html#add)添加小部件和其他UI组件，则请记住，这些组件需要额外的逻辑才能从一个视图持久到另一个视图。
- **小**[部件](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Widget.html) -API中的所有[小部件](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Widget.html)都绑定到特定的视图。如果需要将小部件从2D视图持久化到3D，则需要包括额外的逻辑以切换每个小部件引用的视图。这包括[视图的弹出](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html#popup)实例。

### 相关样品

- [同步MapView和SceneView](https://developers.arcgis.com/javascript/latest/sample-code/views-synchronize/index.html)
- [创建一个Calcite Maps Bootstrap应用程序](https://developers.arcgis.com/javascript/latest/sample-code/frameworks-bootstrap/index.html)

## 12、复合图层

![1573632302594](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573632302594.png)

此示例演示如何创建具有三个复合[MapView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)的应用程序，每个复合[MapView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)具有不同的[空间参考](https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-SpatialReference.html)。主视图用于显示美国48个连续的县，而其他两个视图则显示在阿拉斯加和夏威夷的县。将光标移到一个县上可以查看有关它的信息。

每个视图使用相同的地图实例，其中包含一个[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)。FeatureLayers在几种方面是动态的，包括支持将要素投影到不同的[空间参考](https://developers.arcgis.com/javascript/latest/api-reference/esri-geometry-SpatialReference.html)。为此，将添加`map`到视图中，并引用将在DOM中呈现视图的容器。然后将视图的空间参考设置为所需的坐标系。在下面的代码段中，我们为阿拉斯加创建插图。因此，我们选择将数据投影到阿拉斯加极地立体坐标系，并设置范围以使阿拉斯加充满视图。

```
var akView = new MapView({
  container: "akViewDiv",
  map: map,
  extent: {
    // autocasts as new Extent()
    xmin: 396381,
    ymin: -2099670,
    xmax: 3393803,
    ymax: 148395,
    spatialReference: {
      wkid: 5936
    }
  },
  spatialReference: {
    // WGS 1984 Alaska Polar Stereographic
    // projected coordinate system
    wkid: 5936
  },
  ui: {
    // clears all default widgets from the
    // inset view
    components: []
  }
});
// Add the alaska view container as an inset view
mainView.ui.add("akViewDiv", "bottom-left");
```

默认情况下，视图将使用底图的空间参考。如果未指定底图（例如在此示例中），则使用第一个操作层的空间参考。将视图的空间参考明确设置为其他坐标系会覆盖操作层的空间参考。该示例中图层的空间参考是WGS-84 Web墨卡托辅助球。请参见下图，比较了这两个空间参考之间阿拉斯加的视觉差异。

| 阿拉斯加极地立体照相                                         | 网络墨卡托                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![views-composite-views-ak-polar](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\views-composite-views-ak-polar.png) | ![views-composite-views-ak-wm](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\views-composite-views-ak-wm.png) |

动态重新投影的功能使FeatureLayer与其他静态层（例如[VectorTileLayer）分离](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-VectorTileLayer.html)，该静态层仅显示具有烹饪数据的空间参考的数据。

## 13、全局模式地下导航

此示例演示如何改变[不透明度](https://developers.arcgis.com/javascript/latest/api-reference/esri-Ground.html#opacity)和[颜色](https://developers.arcgis.com/javascript/latest/api-reference/esri-Ground.html#surfaceColor)的的[地面](https://developers.arcgis.com/javascript/latest/api-reference/esri-Ground.html)，以及如何使地下导航在全球[WebScene](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)。

在全局场景中，默认情况下会限制用户在地面上[导航](https://developers.arcgis.com/javascript/latest/api-reference/esri-Ground.html#navigationConstraint)，因此将[navigationConstraint](https://developers.arcgis.com/javascript/latest/api-reference/esri-Ground.html#navigationConstraint)设置为`none`会允许用户在地下导航。

```
map.ground.navigationConstraint = {
  type: "none"
};
```

有时，在不导航至地面之下的情况下查看什么数据位于地面之下会很有用。将地面[不透明度](https://developers.arcgis.com/javascript/latest/api-reference/esri-Ground.html#opacity)设置为低于`1`允许用户透视地面的值。不透明度也会应用于底图（如果有）。

```
map.ground.opacity = 0.6;
```

在此示例中，我们在街道级别上可视化场景。这就是为什么实际上不需要底图的原因。为了避免默认使用网格，我们在地面上设置了[surfaceColor](https://developers.arcgis.com/javascript/latest/api-reference/esri-Ground.html#surfaceColor)：

```
map.ground.surfaceColor = "#CFC7BC";
```

# 三、layer图层

> 各种图层种类

## 1、从门户项目创建图层

这张地图描绘了各县的美国贫困和政治倾向。它是通过从现有[门户项目](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-PortalItem.html)创建[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)实例并将其添加到[SceneView中](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)的基本[Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)来实现的。将项目作为图层加载时，保存到门户中该图层的[Renderer](https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html)和[PopupTemplate](https://developers.arcgis.com/javascript/latest/api-reference/esri-PopupTemplate.html)保留在应用程序中。

要从门户网站加载层，必须使用其门户网站ID。

```
// Creates a layer from a Portal layer item id
Layer.fromPortalItem({
  portalItem: {
    id: "af1ad38816814b7eba3fe74a3b84412d"
  }
}).then(function(layer) {
  // Adds layer to the map
  map.add(layer);
});
```

此应用程序中图层的渲染器使用多个可视变量，这些可视变量在ArcGIS Online Map Viewer中设置并保存到图层。因此，无需在代码中设置自定义渲染器。

请参阅“ [使用ArcGIS Platform”](https://developers.arcgis.com/javascript/latest/guide/working-with-platform/index.html)以获取有关ArcGIS API for JavaScript如何利用门户项目的信息。

## VectorTileLayer

![1573632961315](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573632961315.png)

此示例演示如何通过设置图层的[style](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-VectorTileLayer.html#style)属性从[JSON样式对象](https://www.mapbox.com/mapbox-gl-js/style-spec/)添加[VectorTileLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-VectorTileLayer.html)。简化了Esri的[世纪中期](https://www.arcgis.com/home/item.html?id=7675d44bb1e4428aa2c30a9b68f97822)矢量拼贴样式，并从修改后的JSON对象添加了VectorTileLayer。除上述内容外，Esri还提供了一组具有不同样式的[矢量底图](https://www.arcgis.com/home/group.html?id=30de8da907d240a0bccd5ad3ff25ef4a#overview)。

它还演示了如何在运行时更改2D [MapView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)中VectorTileLayer的[currentStyleInfo.style](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-VectorTileLayer.html#currentStyleInfo)中指定[样式层的](https://www.mapbox.com/mapbox-gl-js/style-spec/#layers)[布局](https://www.mapbox.com/mapbox-gl-js/style-spec/#layer-layout)和[绘制](https://www.mapbox.com/mapbox-gl-js/style-spec/#layer-paint)属性，而不必重新加载整个VectorTileLayer。这可以通过使用4.10版中新增的[setLayoutProperties（）](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-VectorTileLayer.html#setLayoutProperties)和[setPaintProperties（）](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-VectorTileLayer.html#setPaintProperties)方法来完成。

所述[的ArcGIS矢量瓷砖样式编辑器](https://developers.arcgis.com/vector-tile-style-editor/)用于设计定制矢量底图，它利用了上述方法。

```js
// change the font case for the countries(Admin0 point/large) labels layer
document.getElementById("layoutButton").onclick = function() {
  // get the layout properties for the Admin0 point/large layer
  const layoutProperties = vtLayer.getLayoutProperties("Admin0 point/large");

  // change the text-transform layout property
  layoutProperties["text-transform"] =
    layoutProperties["text-transform"] == "uppercase" ? "none" : "uppercase";
  vtLayer.setLayoutProperties("Admin0 point/large", layoutProperties);
};

// change the fill-color for the water(Marine area/1) layer
document.getElementById("paintButton").onclick = function() {
  // get the paint properties for the marine area/1 layer
  const paintProperties = vtLayer.getPaintProperties("Marine area/1");

  // change the fill-color paint property for the layer.
  paintProperties["fill-color"] =
    paintProperties["fill-color"] == "#93CFC7" ? "#0759C1" : "#93CFC7";
  vtLayer.setPaintProperties("Marine area/1", paintProperties);
};
```

具体图层查看官方文档，这里就不做过多翻译。

# 四、FeatureLayer(2D标注常用图层)

![1573633862771](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573633862771.png)

这张地图显示了沃伦·威尔逊学院的树木碳储量。通过向地图添加[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)来实现。默认情况下，它使用要素服务定义的[渲染器](https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html)来[渲染](https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-Renderer.html)要素。您可以通过设置FeatureLayer 的[renderer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#renderer)属性来自定义此属性。 **此功能会大量运用于各个项目**

## 1、从图形数组创建

此示例显示如何从客户端图形创建[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)。这些图形是根据从一组图像的[EXIF](https://en.wikipedia.org/wiki/Exif)数据中提取的GPS数据创建的。

使用客户端图形创建FeatureLayer需要执行以下步骤：

1. 在[FeatureLayer.source](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#source)属性上设置图形数组。所有图形必须具有相同的几何类型。
2. 指定一个[字段](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#fields)对象数组，这些对象提供每个属性字段的架构（名称，别名和类型）。
3. 将[objectID](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#objectIdField)字段属性设置为包含该`source`属性中每个功能的唯一ID的字段。

一旦在图层上设置了上面概述的要求，就可以设置其他图层属性（例如[renderer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#renderer)和[popupTemplate](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#popupTemplate)），以允许您利用FeatureLayer的优点，例如客户端查询和快速的视觉更新。

```js
const layer = new FeatureLayer({
  source: graphics,  // array of graphics objects
  objectIdField: "OBJECTID",
  fields: [{
    name: "OBJECTID",
    type: "oid"
  }, {
    name: "url",
    type: "string"
  }],
  popupTemplate: {
    content: "<img src='{url}'>"
  },
  renderer: {  // overrides the layer's default renderer
    type: "simple",
    symbol: {
      type: "text",
      color: "#7A003C",
      text: "\ue661",
      font: {
        size: 20,
        family: "CalciteWebCoreIcons"
      }
    }
  }
});

map.add(layer);
```

## 2、重点功能

![1573635912650](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573635912650.png)

此示例显示如何突出显示图层中的要素。在功能[SceneLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-SceneLayer.html)，[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)，[CSVLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-CSVLayer.html)，[GeoRSSLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-GeoRSSLayer.html)和[GraphicsLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-GraphicsLayer.html)可以突出。

要突出显示功能，调用[亮点](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-FeatureLayerView.html#highlight)功能的相应的方法[layerView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-LayerView.html)与功能或其`objectID`作为参数。将要素作为参数传递时，它们需要具有`objectID`。

```js
view.whenLayerView(layer).then(function(layerView) {
  // if a feature is already highlighted, then remove the highlight
  if (highlightSelect) {
    highlightSelect.remove();
  }

  // set the highlight on the first feature returned by the query
  highlightSelect = layerView.highlight(feature);
});
```

## 3、使用Arcade表达式标注要素

此示例演示如何使用[Arcade](https://developers.arcgis.com/javascript/latest/guide/arcade/index.html)表达式在[FeatureLayer中](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)标记[要素](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)。Arcade是一种简单，轻量级的脚本语言，可以在运行时评估表达式。您可以使用全局变量在Arcade中访问要素属性。例如，标签城市设立了场，您可以通过以下方式做到这一点：。Arcade提供了一系列内置函数，使您可以在表达式内执行数学计算和逻辑运算。表达式的最后一行必须计算为字符串或数字。`$feature``CITY_NAME``$feature.CITY_NAME`

例如，该示例使用[当（）函数](https://developers.arcgis.com/arcade/function-reference/logical_functions#when)重新分类风向值要么`N`，`NE`，`E`，`SE`，`S`，`SW`，`W`，或`NW`。风向表达式的最后一行作为标签文本返回。要阅读有关Arcade及其语法的更多详细信息，请参阅[Arcade指南页面](https://developers.arcgis.com/javascript/latest/guide/arcade/index.html)。

有关更多信息和已知限制，请参见“ [标签指南”页面](https://developers.arcgis.com/javascript/latest/guide/labeling/index.html)。

## 4、将标签添加到FeatureLayer2

![1573636381492](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573636381492.png)

此示例演示如何在[MapView中](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)向[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)添加标签。您可以通过设置FeatureLayer 的[labelInfo属性](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#labelingInfo)或通过使用已定义的标签加载[PortalItem](https://developers.arcgis.com/javascript/latest/api-reference/esri-portal-PortalItem.html)来显示标签。

`MARKER_ACTIVITY`FeatureLayer字段的值用于标记视图中的每个要素，该视图显示了美国的森林服务娱乐机会。

要将标签添加到图层，首先我们创建[LabelClass](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-support-LabelClass.html)并为各种属性（例如color和[font）](https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-Font.html)分配值。有关更多信息和已知限制，请参见“ [标签指南”页面](https://developers.arcgis.com/javascript/latest/guide/labeling/index.html)。

```js
const labelClass = {
  // autocasts as new LabelClass()
  symbol: {
    type: "text",  // autocasts as new TextSymbol()
    color: "green",
    haloColor: "black",
    font: {  // autocast as new Font()
      family: "Playfair Display",
      size: 12,
      weight: "bold"
    }
  },
  labelPlacement: "above-center",
  labelExpressionInfo: {
    expression: "$feature.MARKER_ACTIVITY"
  }
});
```

该示例从[ArcGIS Online](https://www.arcgis.com/home/index.html)加载PortalItem ，然后将label类添加到`labelingInfo`属性，并应用渲染器。

```js
map.layers = [
  new FeatureLayer({
    portalItem: {
      // autocasts as new PortalItem
      id: "6012738cd1c74582a5f98ea30ae9876f"
    },
    labelingInfo: [labelClass]
  })
];
```

## 5、添加多行标签

![1573636678904](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573636678904.png)

该示例演示了如何使用[Arcade](https://developers.arcgis.com/javascript/latest/guide/arcade/index.html)表达式在2D [FeatureLayer中](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)标注[要素](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)。要显示标签，请将FeatureLayer 的[labelInfo属性](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#labelingInfo)设置为一个或多个标签类。

所有标签表达式都是用[Arcade](https://developers.arcgis.com/javascript/latest/guide/arcade/index.html)编写的，它使您可以通过`$feature`全局变量访问要素属性。标签表达式是在单独的脚本元素中定义的，并使用“ [连接](https://developers.arcgis.com/arcade/function-reference/text_functions/) Arcade”函数设置格式。

使用[TextFormatting.NewLine](https://developers.arcgis.com/arcade/function-reference/constants/#textformattingnewline) Arcade常数将标签分成多行。有关更多信息和已知限制，请参见“ [标签指南”页面](https://developers.arcgis.com/javascript/latest/guide/labeling/index.html)。

```js
<script type="text/plain" id="label">
  // field storing wind direction as a number from 0 - 360
  var DEG = $feature.WIND_DIRECT;
  // field storing wind speed in mph
  var SPEED = $feature.WIND_SPEED;

  // The When() function will classify the wind direction
  // to a compass point (e.g. N, NW, S, SE)
  var DIR = When( SPEED == 0, null,
    (DEG < 22.5 && DEG >= 0) || DEG > 337.5, 'N',
     DEG >= 22.5 && DEG < 67.5, 'NE',
     DEG >= 67.5 && DEG < 112.5, 'E',
     DEG >= 112.5 && DEG < 157.5, 'SE',
     DEG >= 157.5 && DEG < 202.5, 'S',
     DEG >= 202.5 && DEG < 247.5, 'SW',
     DEG >= 247.5 && DEG < 292.5, 'W',
     DEG >= 292.5 && DEG < 337.5, 'NW', null );
  var WIND = SPEED + ' mph ' + DIR;
  var TEMP = Round($feature.TEMP) + '° F';
  var RH = $feature.R_HUMIDITY + '% RH';
  var NAME = $feature.STATION_NAME;

  var labels = [ NAME, TEMP, WIND, RH ];

  // Concatenate all label variables in the same string
  // but on multiple lines
  return Concatenate(labels, TextFormatting.NewLine);
</script>
```

通过`labelingInfo`以下方式引用Arcade表达式。

```js
labelingInfo: [
  {
    labelExpressionInfo: {
      expression: document.getElementById("label").text
    },
    labelPlacement: "center-right",
    minScale: minScale,
    symbol: {
      type: "text",
      font: {
        size: 9,
        family: "Noto Sans"
      },
      horizontalAlignment: "left",
      color: "#2b2b2b"
    }
  }
];
```

## 6、将多个标签类添加到图层

![1573637058079](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573637058079.png)

此示例演示如何在2D [MapView中](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)使用多个[标签类](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-support-LabelClass.html)对[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)进行[标签](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-support-LabelClass.html)。

标签表达式始终使用[Arcade](https://developers.arcgis.com/javascript/latest/guide/arcade/index.html)构造。Arcade提供了一系列[内置函数](https://developers.arcgis.com/arcade/function-reference/)，使您可以在表达式内执行数学计算和逻辑运算。此示例中使用了多个标签类，以视觉上有吸引力的样式显示有关要素的信息。有关更多信息和已知限制，请参见“ [标签指南”页面](https://developers.arcgis.com/javascript/latest/guide/labeling/index.html)。

该示例使用五种标签类别显示了世界各地气象站的天气状况。最初，将地图缩小以显示多个气象站，仅显示温度标签类别。应用程序右上方的“ [书签”小部件](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Bookmarks.html)可让您快速放大以查看所有标签类别。

首先，在`above-left`相对于特征的位置显示温度。我们采用一些`where`逻辑以便以红色显示32度及以上的温度，以蓝色显示32度以下的温度。然后，在该`above-right`位置显示风速和风向，在该位置显示相对湿度，在该`below-left`位置显示气象站名称`below-right`。这两个温度标签类没有设置[minScale](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-support-LabelClass.html#minScale)属性值，而其他三个标签类的[minScale](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-support-LabelClass.html#minScale)属性值为2500000，因此当您进行缩小时，只有温度标签才可见。

```js
const tempArcade = "Round($feature.TEMP) + '° F';";
const lowTempClass = {
  labelExpressionInfo: {
    expression: tempArcade
  },
  labelPlacement: "above-left",
  where: "TEMP <= 32"
};
const highTempClass = {
  labelExpressionInfo: {
    expression: tempArcade
  },
  labelPlacement: "above-left",
  where: "TEMP > 32"
};

const windArcade = document.getElementById("wind-direction").text;
const windClass = {
  labelExpressionInfo: {
    expression: windArcade
  },
  labelPlacement: "above-right",
  minScale: minScale
};

const humidityArcade = "$feature.R_HUMIDITY + '% RH'";
const humidityClass = {
  labelExpressionInfo: {
    expression: humidityArcade
  },
  labelPlacement: "below-left",
  minScale: minScale
};

const nameArcade = "$feature.STATION_NAME";
const nameClass = {
  labelPlacement: "below-right",
  labelExpressionInfo: {
    expression: nameArcade
  },
  minScale: minScale
};

layer.labelingInfo = [nameClass, humidityClass, lowTempClass, highTempClass, windClass];
```

该样品也使用 [When() function](https://developers.arcgis.com/arcade/function-reference/logical_functions#when) 重新分类风向值要么`N`，`NE`，`E`，`SE`，`S`，`SW`，`W`，或`NW`。风向表达式的最后一行作为标签文本返回。要阅读有关Arcade及其语法的更多详细信息，请参阅[Arcade指南页面](https://developers.arcgis.com/javascript/latest/guide/arcade/index.html)。

7、按属性过滤特征

![1573637176797](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573637176797.png)

此示例演示如何在客户端上按属性过滤功能。这可以通过创建一个新的[FeatureFilter](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-support-FeatureFilter.html)并指定其[where](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-support-FeatureFilter.html#where)子句，然后将滤镜对象应用于图层视图的[filter](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-FeatureLayerView.html#filter)属性来完成。

## 这个怎么运作

样本按发出警报的季节过滤洪水警报。要查看不同季节发出的洪水警报，请单击`filter`按钮，然后选择一个季节。

```js
// User clicked on Winter, Spring, Summer or Fall
// set an attribute filter on flood warnings layer view
// to display the warnings issued in that season
function filterBySeason(event) {
  const selectedSeason = event.target.getAttribute("data-season");
    floodLayerView.filter = {
      where: "Season = '" + selectedSeason + "'"
    };
}
```

## 7、将效果应用于特征

![1573637460296](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573637460296.png)

该示例演示了如何使用[空间关系](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-support-FeatureFilter.html#spatialRelationship)来发现要素在空间上的相互关系。它在图层上设置[效果](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-support-FeatureEffect.html)以显示要素符合空间滤镜，同时使不符合空间滤镜的要素变灰。这可以通过创建一个新的`FeatureEffect`并指定其[filter](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-support-FeatureEffect.html#filter)和[excludeEffect](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-support-FeatureEffect.html#excludedEffect)属性来完成。然后可以将效果对象应用于图层视图的[效果](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-layers-FeatureLayerView.html#effect)属性。

## 这个怎么运作

应用程序加载后，用户可以选择：

- 图层-在上设置空间过滤器
- 预定义的几何-查看图层中的要素在空间上如何与此几何相关
- 空间关系-指定要在几何之间看到的空间关系的类型
- 距离-在预定义的几何体周围创建缓冲区，并使用该缓冲区进行空间过滤
- 单位-指定要应用于缓冲区的距离单位

当用户进行选择以显示空间过滤器中包含的要素，同时使从空间过滤器中排除的要素变暗时，将更新该图层。

```js
// set the geometry filter on the visible FeatureLayerView
function updateFilter() {
  featureFilter = {
    // autocasts to FeatureFilter
    geometry: filterGeometry,
    spatialRelationship: geometryRel,
    distance: distance,
    units: unit
  };
  // set effect on excluded features
  // make them gray and transparent
  if (featureLayerView) {
    featureLayerView.effect = {
      filter: featureFilter,
      excludedEffect: "grayscale(100%) opacity(30%)"
    };
  }
}
```

## 8、FeatureLayer的基本查询

![1573637635472](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573637635472.png)此示例演示如何通过使用[queryFeatures（）](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#queryFeatures)方法在旧金山的犯罪数据上查询FeatureLayer 。这允许用户设置查询参数，并通过相应的[Popup](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html)显示查询的响应。

## 这个怎么运作

当应用程序启动时，UI将显示查询选项，可以是基本查询，也可以是按距离查询。根据用户在地图上单击的位置调用查询。在应用程序中单击位置后，将调用此函数。该[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)具有用于其数据进行查询的许多方法。在[（）queryFeatures](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#queryFeatures)方法允许用户查询在FeatureLayer设有基于输入上[的查询](https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-support-Query.html)对象。此方法返回[FeatureSet ](https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-support-FeatureSet.html)[Promise](https://developers.arcgis.com/javascript/latest/guide/programming-patterns/#promises)，可以使用进行访问`.then()`。

```js
function queryFeatures(screenPoint) {
  const point = view.toMap(screenPoint);
  layer.queryFeatures({
    //query object
    geometry: point,
    spatialRelationship: "intersects",
    returnGeometry: false,
    outFields: ["*"],
  })
  .then(function(featureSet) {
    // set graphic location to mouse pointer and add to mapview
    pointGraphic.geometry = point;
    view.graphics.add(pointGraphic);
    // open popup of query result
    view.popup.open({
      location: point,
      features: featureSet.features
    });
  });
}
```

如果用户选择“按距离查询”，则会将两个参数[distance](https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-support-Query.html#distance)和[units](https://developers.arcgis.com/javascript/latest/api-reference/esri-tasks-support-Query.html#units)添加到作为输入的查询对象中`layer.queryFeatures()`，返回距用户单击地图0.5英里以内的要素中的任何项。突出显示的第一个功能不一定与最初单击的功能相同，但是可以通过单击弹出式窗口底部的箭头来查看查询产生的每个功能。

```js
layer.queryFeatures({
  geometry: point,
  // distance and units will be null if basic query selected
  distance: 0.5,
  units: "miles",
  spatialRelationship: "intersects",
  returnGeometry: false,
  outFields: ["*"],
})
```

另请参阅[从FeatureLayer查询特征](https://developers.arcgis.com/javascript/latest/sample-code/featurelayer-query/index.html)。

## 9、从shapefile创建

![1573637815145](E:\学习文档\Myblog\text\source\_posts\ArcGis官方文档中文\1573637815145.png)

此示例显示如何从shapefile 创建客户端[FeatureLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)。

要运行该示例，请使用`Expand`右上角的小部件下载shapefile ，然后添加压缩文件。

请参阅[使用客户端图形创建FeatureLayer](https://developers.arcgis.com/javascript/latest/sample-code/layers-featurelayer-collection/index.html)示例，以获取有关从其他数据源创建FeatureLayer的更多信息。