---
title: WebGis
toc: true
comments: true #是否可评论
archives:  #归档
- WebGis
categories:  #分类
- WebGis
tags:   #标签
- WebGis
- map
---


## Map

API的中心类-用于在页面上创建地图并进行操作。

### 使用范例

```
// initialize the map on the "map" div with a given center and zoom
var map = L.map('map', {
    center: [51.505, -0.09],
    zoom: 13
});
```

### 创建

| Factory                                                  | 描述                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| `L.map(<String> *id*, <Map options>*options?*)`      | 给定一个`<div>`元素的DOM ID实例化一个地图对象，还可以使用`Map options`. |
| `L.map(<HTMLElement> *el*, <Map options>*options?*)` | 给定一个`<div>`HTML元素的实例，并可选地使用一个对象常量来实例化地图对象`Map options`。 |

### Options

| 选项               | 类型      | 默认    | 描述                                                         |
| ------------------ | --------- | ------- | ------------------------------------------------------------ |
| `preferCanvas` | `Boolean` | `false` | Whether [`Path`](https://leafletjs.com/reference-1.2.0.html#path)s should be rendered on a [`Canvas`](https://leafletjs.com/reference-1.2.0.html#canvas) renderer. By default, all `Path`s are rendered in a [`SVG`](https://leafletjs.com/reference-1.2.0.html#svg) renderer. |

#### 控制选项

| 选项                     | 类型      | 默认   | 描述                                                         |
| ------------------------ | --------- | ------ | ------------------------------------------------------------ |
| `attributionControl` | `Boolean` | `true` | 默认情况下是否将[归因控件](https://leafletjs.com/reference-1.2.0.html#control-attribution)添加到地图。 |
| `zoomControl`        | `Boolean` | `true` | Whether a [zoom control](https://leafletjs.com/reference-1.2.0.html#control-zoom) is added to the map by default. |

#### Interaction Options

| 选项                    | 类型             | 默认   | 描述                                                         |
| ----------------------- | ---------------- | ------ | ------------------------------------------------------------ |
| `closePopupOnClick` | `Boolean`        | `true` | `false`如果您不希望用户单击地图时弹出窗口关闭，则将其设置为。 |
| `zoomSnap`          | `Number`         | `1`    | 强制地图的缩放级别始终是此级别的倍数，尤其是在[`fitBounds()`](https://leafletjs.com/reference-1.2.0.html#map-fitbounds)或缩放后立即缩放。默认情况下，缩放级别捕捉到最接近的整数。较低的值（例如`0.5`或`0.1`）允许更大的粒度。值`0` 表示缩放级别将在缩放`fitBounds`或缩放后不被捕捉。 |
| `zoomDelta`         | `Number`         | `1`    | Controls how much the map's zoom level will change after a [`zoomIn()`](https://leafletjs.com/reference-1.2.0.html#map-zoomin), [`zoomOut()`](https://leafletjs.com/reference-1.2.0.html#map-zoomout), pressing `+` or `-` on the keyboard, or using the [zoom controls](https://leafletjs.com/reference-1.2.0.html#control-zoom). Values smaller than `1` (e.g. `0.5`) allow for greater granularity. |
| `trackResize`       | `Boolean`        | `true` | 地图是否自动处理浏览器窗口大小调整以更新自身。               |
| `boxZoom`           | `Boolean`        | `true` | Whether the map can be zoomed to a rectangular area specified by dragging the mouse while pressing the shift key. |
| `doubleClickZoom`   | `Boolean|String` | `true` | 按住shift键，是否可以通过双击地图来放大地图和通过双击来缩小地图。如果通过 `'center'`，则双击缩放将缩放到视图的中心，无论鼠标位于何处。 |
| `dragging`          | `Boolean`        | `true` | Whether the map be draggable with mouse/touch or not.        |

#### 地图状态选项

| Option          | 类型           | 默认             | 描述                                                         |
| --------------- | -------------- | ---------------- | ------------------------------------------------------------ |
| `crs`       | `CRS`          | `L.CRS.EPSG3857` | 要使用的[坐标参考系统](https://leafletjs.com/reference-1.2.0.html#crs)。如果不确定是什么意思，请不要更改。 |
| `center`    | `LatLng`       | `undefined`      | 地图的初始地理中心                                           |
| `zoom`      | `Number`       | `undefined`      | 初始地图缩放级别                                             |
| `minZoom`   | `Number`       | `*`              | Minimum zoom level of the map. If not specified and at least one [`GridLayer`](https://leafletjs.com/reference-1.2.0.html#gridlayer) or [`TileLayer`](https://leafletjs.com/reference-1.2.0.html#tilelayer) is in the map, the lowest of their `minZoom`options will be used instead. |
| `maxZoom`   | `Number`       | `*`              | Maximum zoom level of the map. If not specified and at least one [`GridLayer`](https://leafletjs.com/reference-1.2.0.html#gridlayer) or [`TileLayer`](https://leafletjs.com/reference-1.2.0.html#tilelayer) is in the map, the highest of their `maxZoom`options will be used instead. |
| `layers`    | `Layer[]`      | `[]`             | Array of layers that will be added to the map initially      |
| `maxBounds` | `LatLngBounds` | `null`           | 设置此选项后，地图会将视图限制在给定的地理范围内，如果用户尝试在视图外平移，则会将用户弹回。要动态设置限制，请使用 [`setMaxBounds`](https://leafletjs.com/reference-1.2.0.html#map-setmaxbounds)method. |
| `renderer`  | `Renderer`     | `*`              | 在地图上绘制矢量图层的默认方法。[`L.SVG`](https://leafletjs.com/reference-1.2.0.html#svg) 或[`L.Canvas`](https://leafletjs.com/reference-1.2.0.html#canvas)默认情况下取决于浏览器支持。 |

#### Animation Options

| 选项                         | 类型      | 默认   | 描述                                                         |
| ---------------------------- | --------- | ------ | ------------------------------------------------------------ |
| `zoomAnimation`          | `Boolean` | `true` | Whether the map zoom animation is enabled. By default it's enabled in all browsers that support CSS3 Transitions except Android. |
| `zoomAnimationThreshold` | `Number`  | `4`    | 如果缩放差异超过此值，则不会为缩放设置动画。                 |
| `fadeAnimation`          | `Boolean` | `true` | 是否启用平铺淡入淡出动画。默认情况下，除Android外，所有支持CSS3 Transitions的浏览器均启用该功能。 |
| `markerZoomAnimation`    | `Boolean` | `true` | 标记是否使用缩放动画为其缩放动画，如果禁用，标记将在动画长度内消失。默认情况下，除Android外，所有支持CSS3 Transitions的浏览器均启用该功能。 |
| `transform3DLimit`       | `Number`  | `2^23` | 定义CSS转换转换的最大大小。除非Web浏览器在进行较大的放置后将图层放置在错误的位置，否则不应更改默认值`panBy`。 |

#### 平移惯性选项

| 选项                      | 类型      | 默认       | 描述                                                         |
| ------------------------- | --------- | ---------- | ------------------------------------------------------------ |
| `inertia`             | `Boolean` | `*`        | 如果启用此功能，则地图的平移将具有惯性效果，其中地图会在拖动的同时建立动量并继续沿相同方向移动一段时间。在触摸设备上感觉特别好。默认情况下启用，除非在旧的Android设备上运行。 |
| `inertiaDeceleration` | `Number`  | `3000`     | 惯性运动减慢的速率，以像素/秒²为单位。                       |
| `inertiaMaxSpeed`     | `Number`  | `Infinity` | 惯性运动的最大速度，以像素/秒为单位。                        |
| `easeLinearity`       | `Number`  | `0.2`      |                                                              |
| `worldCopyJump`       | `Boolean` | `false`    | 启用此选项后，地图将跟踪您平移到另一世界的“副本”并无缝跳转到原始世界，以便所有叠加层（如标记和矢量层）仍然可见。 |
| `maxBoundsViscosity`  | `Number`  | `0.0`      | 如果`maxBounds`设置为，则此选项将控制在拖动地图时边界的牢固程度。默认值`0.0`允许用户以正常速度拖动到边界外，较高的值将减慢地图拖动到边界外的速度，`1.0`并使边界完全牢固，从而防止用户拖动到边界外。 |

#### 键盘导航选项

| 选项                   | 类型      | 默认   | Description                                                |
| ---------------------- | --------- | ------ | ---------------------------------------------------------- |
| `keyboard`         | `Boolean` | `true` | 使地图可聚焦，并允许用户使用键盘箭头和`+`/ `-`键浏览地图。 |
| `keyboardPanDelta` | `Number`  | `80`   | Amount of pixels to pan when pressing an arrow key.        |

#### 鼠标滚轮选项

| Option                    | 类型             | 默认   | 描述                                                         |
| ------------------------- | ---------------- | ------ | ------------------------------------------------------------ |
| `scrollWheelZoom`     | `Boolean|String` | `true` | Whether the map can be zoomed by using the mouse wheel. If passed `'center'`, it will zoom to the center of the view regardless of where the mouse was. |
| `wheelDebounceTime`   | `Number`         | `40`   | Limits the rate at which a wheel can fire (in milliseconds). By default user can't zoom via wheel more often than once per 40 ms. |
| `wheelPxPerZoomLevel` | `Number`         | `60`   | 多少滚动像素（如[L.DomEvent.getWheelDelta](https://leafletjs.com/reference-1.2.0.html#domevent-getwheeldelta)所报告）表示更改一个完整的缩放级别。较小的值将使车轮缩放更快（反之亦然）。 |

#### 触摸互动选项

| 选项                     | 类型             | 默认   | 描述                                                         |
| ------------------------ | ---------------- | ------ | ------------------------------------------------------------ |
| `tap`                | `Boolean`        | `true` | Enables mobile hacks for supporting instant taps (fixing 200ms click delay on iOS/Android) and touch holds (fired as `contextmenu` events). |
| `tapTolerance`       | `Number`         | `15`   | 用户在触摸期间可以移动手指的最大像素数，可以将其视为有效的点击。 |
| `touchZoom`          | `Boolean|String` | `*`    | 是否可以通过用两根手指触摸拖动来缩放地图。如果通过`'center'`，则无论触摸事件（手指）在何处，它都会缩放到视图的中心。对具有触摸功能的网络浏览器启用，但旧版Android除外。 |
| `bounceAtZoomLimits` | `Boolean`        | `true` | Set it to false if you don't want the map to zoom beyond min/max zoom and then bounce back when pinch-zooming. |

### 事件

#### 图层事件

| 事件                   | 数据                 | 描述                                                         |
| ---------------------- | -------------------- | ------------------------------------------------------------ |
| `baselayerchange ` | `LayersControlEvent` | 通过[图层控件](https://leafletjs.com/reference-1.2.0.html#control-layers)更改基本图层时触发。 |
| `overlayadd `      | `LayersControlEvent` | Fired when an overlay is selected through the [layer control](https://leafletjs.com/reference-1.2.0.html#control-layers). |
| `overlayremove `   | `LayersControlEvent` | 通过[图层控件](https://leafletjs.com/reference-1.2.0.html#control-layers)取消选择叠加层时触发。 |
| `layeradd `        | `LayerEvent`         | Fired when a new layer is added to the map.                  |
| `layerremove `     | `LayerEvent`         | 从地图上删除某些图层时触发                                   |

#### 地图状态变更事件

| 事件                   | 数据          | 描述                                                         |
| ---------------------- | ------------- | ------------------------------------------------------------ |
| `zoomlevelschange` | `Event`       | Fired when the number of zoomlevels on the map is changed due to adding or removing a layer. |
| `resize `          | `ResizeEvent` | 调整地图大小时触发。                                         |
| `unload `          | `Event`       | Fired when the map is destroyed with [remove](https://leafletjs.com/reference-1.2.0.html#map-remove) method. |
| `viewreset `       | `Event`       | 当地图需要重绘其内容时触发（通常发生在地图缩放或加载时）。对于创建自定义叠加层非常有用。 |
| `load `            | `Event`       | 地图初始化时触发（首次设置其中心和缩放比例时）。             |
| `zoomstart `       | `Event`       | 当地图缩放即将更改时触发（例如在缩放动画之前）。             |
| `movestart `       | `Event`       | 当地图视图开始更改时触发（例如，用户开始拖动地图）。         |
| `zoom `            | `Event`       | Fired repeatedly during any change in zoom level, including zoom and fly animations. |
| `move `            | `Event`       | 在地图的任何移动过程中反复触发，包括平移和飞行动画。         |
| `zoomend `         | `Event`       | Fired when the map has changed, after any animations.        |
| `moveend `         | `Event`       | 地图中心停止更改（例如，用户停止拖动地图）时触发。           |

#### 弹出事件

| 事件                | 数据         | 描述                                    |
| ------------------- | ------------ | --------------------------------------- |
| `popupopen `    | `PopupEvent` | Fired when a popup is opened in the map |
| `popupclose `   | `PopupEvent` | 当地图中的弹出窗口关闭时触发            |
| `autopanstart ` | `Event`      | 打开弹出窗口时地图开始自动平移时触发。  |

#### Tooltip events

| 事件                | 数据           | 描述                         |
| ------------------- | -------------- | ---------------------------- |
| `tooltipopen `  | `TooltipEvent` | 在地图上打开工具提示时触发。 |
| `tooltipclose ` | `TooltipEvent` | 地图中的工具提示关闭时触发。 |

#### 定位事件

| 事件                 | 数据            | 描述                                                         |
| -------------------- | --------------- | ------------------------------------------------------------ |
| `locationerror ` | `ErrorEvent`    | 当地理位置（使用[`locate`](https://leafletjs.com/reference-1.2.0.html#map-locate)方法）失败时触发。 |
| `locationfound ` | `LocationEvent` | 当地理定位（使用[`locate`](https://leafletjs.com/reference-1.2.0.html#map-locate)方法）成功进行时触发。 |

#### 互动事件

| Event             | 数据            | 描述                                                         |
| ----------------- | --------------- | ------------------------------------------------------------ |
| `click `      | `MouseEvent`    | 当用户点击（或点击）地图时触发。                             |
| `dblclick `   | `MouseEvent`    | Fired when the user double-clicks (or double-taps) the map.  |
| `mousedown `  | `MouseEvent`    | 当用户在地图上按下鼠标按钮时触发。                           |
| `mouseup `    | `MouseEvent`    | Fired when the user releases the mouse button on the map.    |
| `mouseover `  | `MouseEvent`    | 当鼠标进入地图时触发。                                       |
| `mouseout `   | `MouseEvent`    | Fired when the mouse leaves the map.                         |
| `mousemove `  | `MouseEvent`    | 当鼠标移到地图上时触发。                                     |
| `contextmenu` | `MouseEvent`    | 当用户在地图上按下鼠标右键时触发，从而阻止默认浏览器上下文菜单显示在此事件上是否有侦听器。当用户按住一次触摸（也称为长按）时，也会在移动设备上触发。 |
| `keypress `   | `KeyboardEvent` | 当地图聚焦时用户按下键盘上的键时触发。                       |
| `preclick `   | `MouseEvent`    | 在地图上单击鼠标之前触发（当您希望在任何现有点击处理程序开始运行之前单击时发生某些事情时，此选项很有用）。 |

#### Other Methods

| 事件            | 数据            | 描述                                     |
| --------------- | --------------- | ---------------------------------------- |
| `zoomanim ` | `ZoomAnimEvent` | Fired on every frame of a zoom animation |

### 方法

| 方法                             | 退货       | 描述                                                         |
| -------------------------------- | ---------- | ------------------------------------------------------------ |
| `getRenderer(<Path>*layer*)` | `Renderer` | 返回[`Renderer`](https://leafletjs.com/reference-1.2.0.html#renderer)应该用于呈现给定 的实例[`Path`](https://leafletjs.com/reference-1.2.0.html#path)。它将确保[`renderer`](https://leafletjs.com/reference-1.2.0.html#renderer)尊重地图和路径的选项，并且渲染器确实存在于地图上。 |

#### 图层和控件的方法

| 方法                                                         | 退货      | 描述                                                         |
| ------------------------------------------------------------ | --------- | ------------------------------------------------------------ |
| `addControl(<Control> *control*)`                        | `this`    | Adds the given control to the map                            |
| `removeControl(<Control> *control*)`                     | `this`    | 从地图上删除给定的控件                                       |
| `addLayer(<Layer> *layer*)`                              | `this`    | 将给定图层添加到地图                                         |
| `removeLayer(<Layer> *layer*)`                           | `this`    | 从地图中删除给定的图层。                                     |
| `hasLayer(<Layer> *layer*)`                              | `Boolean` | Returns `true` if the given layer is currently added to the map |
| `eachLayer(<Function> *fn*, <Object> *context?*)`        | `this`    | 遍历地图的各层，可以选择指定迭代器函数的上下文。`map.eachLayer(function(layer){     layer.bindPopup('Hello'); }); ` |
| `openPopup(<Popup> *popup*)`                             | `this`    | 在关闭先前打开的窗口的同时打开指定的弹出窗口（以确保一次只打开一个弹出窗口以确保可用性）。 |
| `openPopup(<String|HTMLElement> *content*, <LatLng>*latlng*, <Popup options> *options?*)` | `this`    | 创建具有指定内容和选项的弹出窗口，然后在地图上的给定点将其打开。 |
| `closePopup(<Popup> *popup?*)`                           | `this`    | 关闭先前用[openPopup](https://leafletjs.com/reference-1.2.0.html#map-openpopup)（或给定的）打开的弹出窗口。 |
| `openTooltip(<Tooltip> *tooltip*)`                       | `this`    | 打开指定的工具提示。                                         |
| `openTooltip(<String|HTMLElement> *content*,<LatLng> *latlng*, <Tooltip options> *options?*)` | `this`    | 创建具有指定内容和选项的工具提示，然后将其打开。             |
| `closeTooltip(<Tooltip> *tooltip?*)`                     | `this`    | Closes the tooltip given as parameter.                       |

#### 修改地图状态的方法

| 方法                                                         | 退货   | 描述                                                         |
| ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| `setView(<LatLng> *center*, <Number>*zoom*, <Zoom/pan options>*options?*)` | `this` | Sets the view of the map (geographical center and zoom) with the given animation options. |
| `setZoom(<Number> *zoom*,<Zoom/pan options> *options?*)` | `this` | 设置地图的缩放比例。                                         |
| `zoomIn(<Number> *delta?*,<Zoom options> *options?*)`    | `this` | Increases the zoom of the map by `delta` ([`zoomDelta`](https://leafletjs.com/reference-1.2.0.html#map-zoomdelta) by default). |
| `zoomOut(<Number> *delta?*,<Zoom options> *options?*)`   | `this` | Decreases the zoom of the map by `delta` ([`zoomDelta`](https://leafletjs.com/reference-1.2.0.html#map-zoomdelta) by default). |
| `setZoomAround(<LatLng> *latlng*,<Number> *zoom*, <Zoom options>*options*)` | `this` | Zooms the map while keeping a specified geographical point on the map stationary (e.g. used internally for scroll zoom and double-click zoom). |
| `setZoomAround(<Point> *offset*,<Number> *zoom*, <Zoom options>*options*)` | `this` | 缩放地图，同时使地图上的指定像素（相对于左上角）保持不变。   |
| `fitBounds(<LatLngBounds> *bounds*,<fitBounds options> *options?*)` | `this` | Sets a map view that contains the given geographical bounds with the maximum zoom level possible. |
| `fitWorld(<fitBounds options>*options?*)`                | `this` | 设置一个地图视图，该视图主要包含可能具有最大缩放级别的整个世界。 |
| `panTo(<LatLng> *latlng*,<Pan options> *options?*)`      | `this` | Pans the map to a given center.                              |
| `panBy(<Point> *offset*,<Pan options> *options?*)`       | `this` | Pans the map by a given number of pixels (animated).         |
| `flyTo(<LatLng> *latlng*, <Number>*zoom?*, <Zoom/pan options>*options?*)` | `this` | 设置执行平滑的平移缩放动画的地图视图（地理中心和缩放）。     |
| `flyToBounds(<LatLngBounds>*bounds*, <fitBounds options>*options?*)` | `this` | Sets the view of the map with a smooth animation like [`flyTo`](https://leafletjs.com/reference-1.2.0.html#map-flyto), but takes a bounds parameter like [`fitBounds`](https://leafletjs.com/reference-1.2.0.html#map-fitbounds). |
| `setMaxBounds(<Bounds> *bounds*)`                        | `this` | Restricts the map view to the given bounds (see the [maxBounds](https://leafletjs.com/reference-1.2.0.html#map-maxbounds) option). |
| `setMinZoom(<Number> *zoom*)`                            | `this` | 设置可用缩放级别的下限（请参见[minZoom](https://leafletjs.com/reference-1.2.0.html#map-minzoom)选项）。 |
| `setMaxZoom(<Number> *zoom*)`                            | `this` | 设置可用缩放级别的上限（请参见[maxZoom](https://leafletjs.com/reference-1.2.0.html#map-maxzoom)选项）。 |
| `panInsideBounds(<LatLngBounds>*bounds*, <Pan options> *options?*)` | `this` | 将地图平移到位于给定范围内（如果尚未存在）的最近视图，并使用特定选项（如果有）控制动画。 |
| `invalidateSize(<Zoom/Pan options>*options*)`            | `this` | 检查地图容器的大小是否更改，如果是，则更新地图-在动态更改地图大小后调用它，默认情况下还设置动画平移。如果`options.pan`为`false`，则不会发生平移。如果`options.debounceMoveend`为is `true`，它将延迟`moveend`事件，以便即使连续多次调用该方法也不会经常发生。 |
| `invalidateSize(<Boolean> *animate*)`                    | `this` | Checks if the map container size changed and updates the map if so — call it after you've changed the map size dynamically, also animating pan by default. |
| `stop()`                                                 | `this` | Stops the currently running `panTo` or `flyTo` animation, if any. |

#### 地理位置定位方法

| 方法                                     | 退货   | Description                                                  |
| ---------------------------------------- | ------ | ------------------------------------------------------------ |
| `locate(<Locate options>*options?*)` | `this` | 尝试使用Geolocation API定位用户，[`locationfound`](https://leafletjs.com/reference-1.2.0.html#map-locationfound) 并在成功或失败时触发包含位置数据的事件[`locationerror`](https://leafletjs.com/reference-1.2.0.html#map-locationerror) event on failure, and optionally sets the map view to the user's location with respect to detection accuracy (or to the world view if geolocation failed). Note that, if your page doesn't use HTTPS, this method will fail in modern browsers ([Chrome 50 and newer](https://sites.google.com/a/chromium.org/dev/Home/chromium-security/deprecating-powerful-features-on-insecure-origins)) See [`Locate options`](https://leafletjs.com/reference-1.2.0.html#locate-options) for more details. |
| `stopLocate()`                       | `this` | `map.locate({watch: true})` 如果使用调用map.locate，则停止观看先前由其发起的位置，并中止重置地图视图 `{setView: true}`。 |

#### 其他方法

| 方法                                                         | 退货          | 描述                                                         |
| ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ |
| `addHandler(<String> *name*,<Function> *HandlerClass*)`  | `this`        | Adds a new [`Handler`](https://leafletjs.com/reference-1.2.0.html#handler) to the map, given its name and constructor function. |
| `remove()`                                               | `this`        | 销毁地图并清除所有相关的事件侦听器。                         |
| `createPane(<String> *name*,<HTMLElement> *container?*)` | `HTMLElement` | 如果给定名称不存在，则创建一个新的[地图窗格](https://leafletjs.com/reference-1.2.0.html#map-pane)，然后将其返回。窗格是作为的子级创建的`container`，如果未设置，则作为主地图窗格的子级创建的。 |
| `getPane(<String|HTMLElement>*pane*)`                    | `HTMLElement` | Returns a [map pane](https://leafletjs.com/reference-1.2.0.html#map-pane), given its name or its HTML element (its identity). |
| `getPanes()`                                             | `Object`      | 返回一个普通对象，该对象包含所有[窗格](https://leafletjs.com/reference-1.2.0.html#map-pane)的名称作为键，并包含所有[窗格的](https://leafletjs.com/reference-1.2.0.html#map-pane)值。 |
| `getContainer()`                                         | `HTMLElement` | 返回包含地图的HTML元素。                                     |
| `whenReady(<Function> *fn*,<Object> *context?*)`         | `this`        | `fn`当地图使用视图（中心和缩放）和至少一层进行初始化时运行给定函数，或者如果地图已经初始化，则立即运行给定函数（可选地传递函数上下文）。 |

#### Methods for Getting Map State

| 方法                                                         | 退货           | 描述                                                         |
| ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| `getCenter()`                                            | `LatLng`       | 返回地图视图的地理中心                                       |
| `getZoom()`                                              | `Number`       | 返回地图视图的当前缩放级别                                   |
| `getBounds()`                                            | `LatLngBounds` | Returns the geographical bounds visible in the current map view |
| `getMinZoom()`                                           | `Number`       | 返回地图的最小缩放级别（如果在`minZoom`地图或任何图层的选项中设置），或者`0`默认情况下。 |
| `getMaxZoom()`                                           | `Number`       | Returns the maximum zoom level of the map (if set in the `maxZoom`option of the map or of any layers). |
| `getBoundsZoom(<LatLngBounds>*bounds*, <Boolean> *inside?*)` | `Number`       | 返回最大缩放级别，指定范围完全适合该缩放级别。如果`inside`（可选）设置为`true`，则该方法将返回最小缩放级别，地图视图将在该最小缩放级别上完全适合给定范围。 |
| `getSize()`                                              | `Point`        | 返回地图容器的当前大小（以像素为单位）。                     |
| `getPixelBounds()`                                       | `Bounds`       | Returns the bounds of the current map view in projected pixel coordinates (sometimes useful in layer and overlay implementations). |
| `getPixelOrigin()`                                       | `Point`        | 返回地图图层左上角点的投影像素坐标（在自定义图层和叠加层实现中很有用）。 |
| `getPixelWorldBounds(<Number>*zoom?*)`                   | `Bounds`       | Returns the world's bounds in pixel coordinates for zoom level `zoom`. If `zoom` is omitted, the map's current zoom level is used. |

#### 转换方式

| 方法                                                      | 退货           | 描述                                                         |
| --------------------------------------------------------- | -------------- | ------------------------------------------------------------ |
| `getZoomScale(<Number> *toZoom*, <Number>*fromZoom*)` | `Number`       | 返回要应用于从缩放级别`fromZoom`到的地图过渡的比例因子 `toZoom`。在内部使用以帮助缩放动画。 |
| `getScaleZoom(<Number> *scale*, <Number>*fromZoom*)`  | `Number`       | 返回地图最终将达到的缩放级别（如果处于`fromZoom` 级别并且所有内容都按比例缩放）`scale`。的逆[`getZoomScale`](https://leafletjs.com/reference-1.2.0.html#map-getZoomScale)。 |
| `project(<LatLng> *latlng*, <Number> *zoom*)`         | `Point`        | [`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)根据地图CRS的投影来投影地理坐标，然后根据`zoom`和CRS 进行缩放[`Transformation`](https://leafletjs.com/reference-1.2.0.html#transformation)。结果是相对于CRS原点的像素坐标。 |
| `unproject(<Point> *point*, <Number> *zoom*)`         | `LatLng`       | 的逆[`project`](https://leafletjs.com/reference-1.2.0.html#map-project)。 |
| `layerPointToLatLng(<Point> *point*)`                 | `LatLng`       | Given a pixel coordinate relative to the [origin pixel](https://leafletjs.com/reference-1.2.0.html#map-getpixelorigin), returns the corresponding geographical coordinate (for the current zoom level). |
| `latLngToLayerPoint(<LatLng> *latlng*)`               | `Point`        | 给定地理坐标，返回相对于[原点像素](https://leafletjs.com/reference-1.2.0.html#map-getpixelorigin)的相应像素坐标。 |
| `wrapLatLng(<LatLng> *latlng*)`                       | `LatLng`       | Returns a [`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng) where `lat` and `lng` has been wrapped according to the map's CRS's `wrapLat` and `wrapLng`properties, if they are outside the CRS's bounds. By default this means longitude is wrapped around the dateline so its value is between -180 and +180 degrees. |
| `wrapLatLngBounds(<LatLngBounds> *bounds*)`           | `LatLngBounds` | 返回[`LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds)大小与给定大小相同的a ，确保其中心在CRS的范围内。默认情况下，这意味着中心经度围绕日期线缠绕，因此其值在-180至+180度之间，并且大多数边界与CRS的边界重叠。 |
| `distance(<LatLng> *latlng1*, <LatLng>*latlng2*)`     | `Number`       | Returns the distance between two geographical coordinates according to the map's CRS. By default this measures distance in meters. |
| `containerPointToLayerPoint(<Point>*point*)`          | `Point`        | 给定相对于地图容器的像素坐标，则返回相对于[原点pixel](https://leafletjs.com/reference-1.2.0.html#map-getpixelorigin)的相应像素坐标。 |
| `layerPointToContainerPoint(<Point>*point*)`          | `Point`        | Given a pixel coordinate relative to the [origin pixel](https://leafletjs.com/reference-1.2.0.html#map-getpixelorigin), returns the corresponding pixel coordinate relative to the map container. |
| `containerPointToLatLng(<Point> *point*)`             | `LatLng`       | 给定相对于地图容器的像素坐标，则返回相应的地理坐标（针对当前缩放级别）。 |
| `latLngToContainerPoint(<LatLng> *latlng*)`           | `Point`        | 给定地理坐标，则返回相对于地图容器的相应像素坐标。           |
| `mouseEventToContainerPoint(<MouseEvent>*ev*)`        | `Point`        | 给定一个MouseEvent对象，返回相对于发生事件的地图容器的像素坐标。 |
| `mouseEventToLayerPoint(<MouseEvent> *ev*)`           | `Point`        | 给定一个MouseEvent对象，返回相对于事件发生的[原始像素](https://leafletjs.com/reference-1.2.0.html#map-getpixelorigin)的像素坐标。 |
| `mouseEventToLatLng(<MouseEvent> *ev*)`               | `LatLng`       | 给定一个MouseEvent对象，返回发生事件的地理坐标。             |

▶ Methods inherited from [Evented](https://leafletjs.com/reference-1.2.0.html#evented)

### 性质

#### 处理程序

| 属性                   | 类型      | Description                                     |
| ---------------------- | --------- | ----------------------------------------------- |
| `boxZoom `         | `Handler` | 框（使用鼠标按住Shift键并拖动）缩放处理程序。   |
| `doubleClickZoom ` | `Handler` | 双击缩放处理程序。                              |
| `dragging `        | `Handler` | Map dragging handler (by both mouse and touch). |
| `keyboard `        | `Handler` | 键盘导航处理程序。                              |
| `scrollWheelZoom ` | `Handler` | 滚轮缩放处理程序。                              |
| `tap `             | `Handler` | 移动触摸骇客（快速点击和触摸保持）处理程序。    |
| `touchZoom `       | `Handler` | 触摸缩放处理程序。                              |

### 地图窗格

窗格是用于控制地图上图层顺序的DOM元素。您可以使用[`map.getPane`](https://leafletjs.com/reference-1.2.0.html#map-getpane)或 [`map.getPanes`](https://leafletjs.com/reference-1.2.0.html#map-getpanes)方法访问窗格。可以使用[`map.createPane`](https://leafletjs.com/reference-1.2.0.html#map-createpane)方法创建新窗格 。每个地图都有以下默认窗格，它们的唯一区别仅在于zIndex。

| 窗格              | 类型          | Z指数    | 描述                                                         |
| ----------------- | ------------- | -------- | ------------------------------------------------------------ |
| `mapPane`     | `HTMLElement` | `'auto'` | Pane that contains all other map panes                       |
| `tilePane`    | `HTMLElement` | `200`    | [`GridLayer`](https://leafletjs.com/reference-1.2.0.html#gridlayer)s和[`TileLayer`](https://leafletjs.com/reference-1.2.0.html#tilelayer)s的窗格 |
| `overlayPane` | `HTMLElement` | `400`    | 矢量叠加层（[`Path`](https://leafletjs.com/reference-1.2.0.html#path)s）的窗格，如[`Polyline`](https://leafletjs.com/reference-1.2.0.html#polyline)s和[`Polygon`](https://leafletjs.com/reference-1.2.0.html#polygon)s |
| `shadowPane`  | `HTMLElement` | `500`    | Pane for overlay shadows (e.g. [`Marker`](https://leafletjs.com/reference-1.2.0.html#marker) shadows) |
| `markerPane`  | `HTMLElement` | `600`    | 窗格[`Icon`](https://leafletjs.com/reference-1.2.0.html#icon)第[`Marker`](https://leafletjs.com/reference-1.2.0.html#marker)小号 |
| `tooltipPane` | `HTMLElement` | `650`    | 工具提示窗格。                                               |
| `popupPane`   | `HTMLElement` | `700`    | [`Popup`](https://leafletjs.com/reference-1.2.0.html#popup)s 窗格。 |

### 找到选项

用于[`Map`](https://leafletjs.com/reference-1.2.0.html#map)输入`options`参数的某些地理位置方法。这是一个普通的javascript对象，具有以下可选组件：

| 选项                     | 类型      | 默认       | 描述                                                         |
| ------------------------ | --------- | ---------- | ------------------------------------------------------------ |
| `watch`              | `Boolean` | `false`    | 如果为`true`，则开始使用W3C `watchPosition`方法连续观察位置变化（而不是检测一次）。您可以稍后使用`map.stopLocate()`方法停止观看 。 |
| `setView`            | `Boolean` | `false`    | If `true`, automatically sets the map view to the user location with respect to detection accuracy, or to world view if geolocation failed. |
| `maxZoom`            | `Number`  | `Infinity` | 使用`setView`选项时自动视图设置的最大变焦。                  |
| `timeout`            | `Number`  | `10000`    | 触发`locationerror`事件之前等待地理位置响应的毫秒数 。       |
| `maximumAge`         | `Number`  | `0`        | 检测到的位置的最大年龄。如果自上次地理位置响应以来经过的时间少于此毫秒数，`locate`则将返回缓存的位置。 |
| `enableHighAccuracy` | `Boolean` | `false`    | 启用高精度，请参阅[W3C规范中的描述](http://dev.w3.org/geo/api/spec-source.html#high-accuracy)。 |

### 缩放选项

某些[`Map`](https://leafletjs.com/reference-1.2.0.html#map)修改缩放级别的方法采用一个`options` 参数。这是一个普通的javascript对象，具有以下可选组件：

| 选项          | 类型      | 默认 | 描述                                                         |
| ------------- | --------- | ---- | ------------------------------------------------------------ |
| `animate` | `Boolean` | ``   | 如果未指定，则如果缩放原点位于当前视图内，则会发生缩放动画。如果为`true`，则无论缩放原点在哪里，地图都会尝试设置缩放动画。设置`false`将使其始终完全重置视图而无需动画。 |

### 平移选项

某些[`Map`](https://leafletjs.com/reference-1.2.0.html#map)修改地图中心的方法采用一个`options` 参数。这是一个普通的javascript对象，具有以下可选组件：

| 选项                | 类型      | 默认    | 描述                                                         |
| ------------------- | --------- | ------- | ------------------------------------------------------------ |
| `animate`       | `Boolean` | ``      | 如果为`true`，则在可能的情况下平移将始终保持动画状态。如果为`false`，则不会为平移设置动画，或者在平移屏幕以外的地方重设地图视图，或者只是为地图窗格设置新的偏移量（除非`panBy` 后者总是这样做）。 |
| `duration`      | `Number`  | `0.25`  | Duration of animated panning, in seconds.                    |
| `easeLinearity` | `Number`  | `0.25`  | 平移动画缓和的曲率因子（[三次贝塞尔曲线的](http://cubic-bezier.com/)第三个参数 ）。1.0表示线性动画，此数字越小，曲线越弯曲。 |
| `noMoveStart`   | `Boolean` | `false` | If `true`, panning won't fire `movestart` event on start (used internally for panning inertia). |

### 缩放/平移选项

▶从[缩放](https://leafletjs.com/reference-1.2.0.html#zoom-options)选项继承的[选项](https://leafletjs.com/reference-1.2.0.html#zoom-options)

▶从[平移](https://leafletjs.com/reference-1.2.0.html#pan-options)选项继承的[选项](https://leafletjs.com/reference-1.2.0.html#pan-options)

### FitBounds选项

| 选项                     | 类型     | Default  | 描述                                                         |
| ------------------------ | -------- | -------- | ------------------------------------------------------------ |
| `paddingTopLeft`     | `Point`  | `[0, 0]` | 设置将视图设置为适合边界时不应考虑的地图容器左上角的填充量。如果您在地图上有一些控制叠加层（如侧边栏）并且不想让它们遮挡要缩放的对象，则很有用。 |
| `paddingBottomRight` | `Point`  | `[0, 0]` | 地图的右下角也一样。                                         |
| `padding`            | `Point`  | `[0, 0]` | 等效于将左上和右下填充都设置为相同的值。                     |
| `maxZoom`            | `Number` | `null`   | 使用的最大可能缩放。                                         |

▶从[缩放](https://leafletjs.com/reference-1.2.0.html#zoom-options)选项继承的[选项](https://leafletjs.com/reference-1.2.0.html#zoom-options)

▶从[平移](https://leafletjs.com/reference-1.2.0.html#pan-options)选项继承的[选项](https://leafletjs.com/reference-1.2.0.html#pan-options)

## 记号笔

L.Marker用于在地图上显示可点击/可拖动的图标。扩展[`Layer`](https://leafletjs.com/reference-1.2.0.html#layer)。

### 使用范例

```
L.marker([50.5, 30.5]).addTo(map);
```

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.marker(<LatLng> *latlng*,<Marker options> *options?*)` | Instantiates a Marker object given a geographical point and optionally an options object. |

### 选件

| 选项                      | 类型      | 默认           | 描述                                                         |
| ------------------------- | --------- | -------------- | ------------------------------------------------------------ |
| `icon`                | `Icon`    | `*`            | 用于渲染标记的图标实例。有关如何自定义标记图标的详细信息，请参见[图标文档](https://leafletjs.com/reference-1.2.0.html#icon)。如果未指定，[`L.Icon.Default`](https://leafletjs.com/reference-1.2.0.html#icon-default)则使用的通用实例。 |
| `draggable`           | `Boolean` | `false`        | Whether the marker is draggable with mouse/touch or not.     |
| `keyboard`            | `Boolean` | `true`         | 是否可以使用键盘将标记设置为选项卡，然后按Enter键单击。      |
| `title`               | `String`  | `''`           | Text for the browser tooltip that appear on marker hover (no tooltip by default). |
| `alt`                 | `String`  | `''`           | `alt`图标图像属性的文本（用于辅助功能）。                    |
| `zIndexOffset`        | `Number`  | `0`            | 默认情况下，标记图像zIndex是根据其纬度自动设置的。如果要将标记放在所有其他标记的上方（或下方），请使用此选项，并指定一个较高的值`1000`（分别为或较高的负值）。 |
| `opacity`             | `Number`  | `1.0`          | 标记的不透明度。                                             |
| `riseOnHover`         | `Boolean` | `false`        | 如果为`true`，则将鼠标悬停在该标记上时，该标记将位于其他标记之上。 |
| `riseOffset`          | `Number`  | `250`          | The z-index offset used for the `riseOnHover` feature.       |
| `pane`                | `String`  | `'markerPane'` | `Map pane` 将在其中添加标记图标的位置。                      |
| `bubblingMouseEvents` | `Boolean` | `false`        | When `true`, a mouse event on this marker will trigger the same event on the map (unless [`L.DomEvent.stopPropagation`](https://leafletjs.com/reference-1.2.0.html#domevent-stoppropagation) is used). |

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

除了[共享层的方法](https://leafletjs.com/reference-1.2.0.html#layer)等`addTo()`，并`remove()`与[弹出方法](https://leafletjs.com/reference-1.2.0.html#popup)等bindPopup（）也可以使用以下方法：

| 事件       | 数据    | 描述                                                         |
| ---------- | ------- | ------------------------------------------------------------ |
| `move` | `Event` | 通过[`setLatLng`](https://leafletjs.com/reference-1.2.0.html#marker-setlatlng)或通过[拖动](https://leafletjs.com/reference-1.2.0.html#marker-dragging)移动标记时触发。新旧坐标包含在事件参数中`oldLatLng`，如，[`latlng`](https://leafletjs.com/reference-1.2.0.html#latlng). |

#### 拖曳事件

| 事件             | 数据           | 描述                                                      |
| ---------------- | -------------- | --------------------------------------------------------- |
| `dragstart ` | `Event`        | Fired when the user starts dragging the marker.           |
| `movestart ` | `Event`        | 当标记开始移动（由于拖动）时触发。                        |
| `drag `      | `Event`        | 用户拖动标记时反复触发。                                  |
| `dragend `   | `DragEndEvent` | 当用户停止拖动标记时触发。                                |
| `moveend `   | `Event`        | Fired when the marker stops moving (because of dragging). |

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法                                     | 退货     | 描述                                                         |
| ---------------------------------------- | -------- | ------------------------------------------------------------ |
| `toGeoJSON()`                        | `Object` | 返回一个 [`GeoJSON`](https://leafletjs.com/reference-1.2.0.html#geojson)标记的表示形式（作为GeoJSON [`Point`](https://leafletjs.com/reference-1.2.0.html#point)功能）。 |
| `getLatLng()`                        | `LatLng` | 返回标记的当前地理位置。                                     |
| `setLatLng(<LatLng> *latlng*)`       | `this`   | 将标记位置更改为给定点。                                     |
| `setZIndexOffset(<Number> *offset*)` | `this`   | Changes the [zIndex offset](https://leafletjs.com/reference-1.2.0.html#marker-zindexoffset) of the marker. |
| `setIcon(<Icon> *icon*)`             | `this`   | 更改标记图标。                                               |
| `setOpacity(<Number> *opacity*)`     | `this`   | Changes the opacity of the marker.                           |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

### Properties

#### 交互处理程序

交互处理程序是标记实例的属性，允许您在运行时控制交互行为，从而启用或禁用某些功能，例如拖动（请参见

`Handler`

方法）。例：

```
marker.dragging.disable();
```

| 属性           | 类型      | 描述                                                         |
| -------------- | --------- | ------------------------------------------------------------ |
| `dragging` | `Handler` | Marker dragging handler (by both mouse and touch). Only valid when the marker is on the map (Otherwise set [`marker.options.draggable`](https://leafletjs.com/reference-1.2.0.html#marker-draggable)). |

## 弹出

用于在地图的某些位置打开弹出窗口。使用[Map.openPopup](https://leafletjs.com/reference-1.2.0.html#map-openpopup)打开弹出窗口，同时确保一次仅打开一个弹出窗口（出于可用性考虑，建议使用），或者使用[Map.addLayer](https://leafletjs.com/reference-1.2.0.html#map-addlayer)打开[任意数量的窗口](https://leafletjs.com/reference-1.2.0.html#map-addlayer)。

### 使用范例

如果您只想将弹出窗口绑定到标记单击，然后将其打开，则非常简单：

```
marker.bindPopup(popupContent).openPopup();
```

折线之类的路径叠加层也有一种`bindPopup`方法。这是在地图上打开弹出窗口的更复杂的方法：

```
var popup = L.popup()
    .setLatLng(latlng)
    .setContent('<p>Hello world!<br />This is a nice popup.</p>')
    .openOn(map);
```

### 创建

| 厂                                                         | 描述                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| `L.popup(<Popup options>*options?*, <Layer>*source?*)` | Instantiates a [`Popup`](https://leafletjs.com/reference-1.2.0.html#popup) object given an optional `options` object that describes its appearance and location and an optional `source` object that is used to tag the popup with a reference to the Layer to which it refers. |

### 选件

| 选项                            | Type      | 默认          | 描述                                                         |
| ------------------------------- | --------- | ------------- | ------------------------------------------------------------ |
| `maxWidth`                  | `Number`  | `300`         | 弹出窗口的最大宽度，以像素为单位。                           |
| `minWidth`                  | `Number`  | `50`          | 弹出窗口的最小宽度，以像素为单位。                           |
| `maxHeight`                 | `Number`  | `null`        | 如果设置，则在其内容超出弹出窗口的范围内创建给定高度的可滚动容器。 |
| `autoPan`                   | `Boolean` | `true`        | `false`如果您不希望地图进行平移动画以适合打开的弹出窗口，则将其设置为。 |
| `autoPanPaddingTopLeft`     | `Point`   | `null`        | 自动平移后，弹出窗口和地图视图的左上角之间的边距。           |
| `autoPanPaddingBottomRight` | `Point`   | `null`        | 执行自动平移后，弹出窗口和地图视图右下角之间的边距。         |
| `autoPanPadding`            | `Point`   | `Point(5, 5)` | Equivalent of setting both top left and bottom right autopan padding to the same value. |
| `keepInView`                | `Boolean` | `false`       | 将它设置为`true`如果要防止用户平移弹出关闭屏幕，而它是开放的。 |
| `closeButton`               | `Boolean` | `true`        | 控制弹出窗口中是否存在关闭按钮。                             |
| `autoClose`                 | `Boolean` | `true`        | 将它设置为`false`，如果你想另一个弹出打开时覆盖弹出关闭的默认行为。 |
| `closeOnClick`              | `Boolean` | `*`           | 如果您要覆盖用户单击地图时关闭弹出窗口的默认行为，请进行设置。默认为地图[`closePopupOnClick`](https://leafletjs.com/reference-1.2.0.html#map-closepopuponclick)选项。 |
| `className`                 | `String`  | `''`          | A custom CSS class name to assign to the popup.              |

▶从[DivOverlay](https://leafletjs.com/reference-1.2.0.html#divoverlay)继承的选项

▶ Options inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### 事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶ Popup events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| Method                                                       | 退货                 | 描述                                                         |
| ------------------------------------------------------------ | -------------------- | ------------------------------------------------------------ |
| `getLatLng()`                                            | `LatLng`             | 返回弹出窗口的地理位置。                                     |
| `setLatLng(<LatLng> *latlng*)`                           | `this`               | 设置打开弹出窗口的地理位置。                                 |
| `getContent()`                                           | `String|HTMLElement` | Returns the content of the popup.                            |
| `setContent(<String|HTMLElement|Function>*htmlContent*)` | `this`               | 设置弹出窗口的HTML内容。如果传递了函数，则源层将传递给该函数。该函数应返回a `String`或`HTMLElement`要在弹出窗口中使用。 |
| `getElement()`                                           | `String|HTMLElement` | 别名 [getContent()](https://leafletjs.com/reference-1.2.0.html#popup-getcontent) |
| `update()`                                               | `null`               | 更新弹出内容，布局和位置。更改内部内容（例如，加载图像）后更新弹出窗口很有用。 |
| `isOpen()`                                               | `Boolean`            | `true`当弹出窗口在地图上可见时返回。                         |
| `bringToFront()`                                         | `this`               | Brings this popup in front of other popups (in the same map pane). |
| `bringToBack()`                                          | `this`               | 将此弹出窗口带到其他弹出窗口的后面（在同一地图窗格中）。     |
| `openOn(<Map> *map*)`                                    | `this`               | 将弹出窗口添加到地图并关闭上一个窗口。与相同`map.openPopup(popup)`。 |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 工具提示

Used to display small texts on top of map layers.

### 使用范例

```
marker.bindTooltip("my tooltip text").openTooltip();
```

关于工具提示偏移的注意事项。Leaflet在计算工具提示偏移时考虑了两个选项：

- 该`offset`工具提示选项：它默认为[0,0]，并且它的特定于一个工具提示。添加一个正的x偏移量以将工具提示移至右侧，并添加一个正的y偏移量以将其移至底部。负数将移至左侧和顶部。
- 该`tooltipAnchor`图标选项：这只会被视为标记。如果使用自定义图标，则应调整此值。

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.tooltip(<Tooltip options>*options?*, <Layer> *source?*)` | 在给定一个`options`描述对象的外观和位置的可选`source`对象以及一个用来引用指向其所引用的图层来标记该工具提示的可选对象的实例化Tooltip对象。 |

### 选件

| 选项              | 类型      | 默认            | Description                                                  |
| ----------------- | --------- | --------------- | ------------------------------------------------------------ |
| `pane`        | `String`  | `'tooltipPane'` | `Map pane` 工具提示将添加到的位置。                          |
| `offset`      | `Point`   | `Point(0, 0)`   | 工具提示位置的可选偏移量。                                   |
| `direction`   | `String`  | `'auto'`        | Direction where to open the tooltip. Possible values are: `right`, `left`, `top`, `bottom`, `center`, `auto`. `auto` will dynamicaly switch between `right` and `left`according to the tooltip position on the map. |
| `permanent`   | `Boolean` | `false`         | Whether to open the tooltip permanently or only on mouseover. |
| `sticky`      | `Boolean` | `false`         | 如果为true，则工具提示将跟随鼠标，而不是固定在功能中心。     |
| `interactive` | `Boolean` | `false`         | 如果为true，则工具提示将侦听功能事件。                       |
| `opacity`     | `Number`  | `0.9`           | Tooltip container opacity.                                   |

▶从[DivOverlay](https://leafletjs.com/reference-1.2.0.html#divoverlay)继承的选项

▶ Options inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### 事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## TileLayer

用于在地图上加载和显示图块图层。扩展[`GridLayer`](https://leafletjs.com/reference-1.2.0.html#gridlayer)。

### 使用范例

```
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png?{foo}', {foo: 'bar'}).addTo(map);
```

#### 网址模板

以下形式的字符串：

```
'http://{s}.somedomain.com/blabla/{z}/{x}/{y}{r}.png'
```

`{s}`装置可用的子域中的一个（顺序使用与每个域限制浏览器并行请求帮助;子域值在选项中指定; `a`，`b`或`c`通过默认，可省略），`{z}`-缩放级别，`{x}`以及`{y}`-瓷砖坐标。`{r}`可用于在网址中添加“ @ 2x”以加载视网膜图块。您可以在模板中使用自定义键，该键将从TileLayer选项进行[评估](https://leafletjs.com/reference-1.2.0.html#util-template)，如下所示：

```
L.tileLayer('http://{s}.somedomain.com/{foo}/{z}/{x}/{y}.png', {foo: 'bar'});
```

### 创建

#### 扩展方式

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.tilelayer(<String> *urlTemplate*,<TileLayer options> *options?*)` | 给定a实例化一个tile图层对象`URL template`，还可以选择一个options对象。 |

### 选件

| 选项               | 类型              | 默认    | 描述                                                         |
| ------------------ | ----------------- | ------- | ------------------------------------------------------------ |
| `minZoom`      | `Number`          | `0`     | 显示该层的最小缩放级别（含）。                               |
| `maxZoom`      | `Number`          | `18`    | 将显示此图层的最大缩放级别（含）。                           |
| `subdomains`   | `String|String[]` | `'abc'` | Subdomains of the tile service. Can be passed in the form of one string (where each letter is a subdomain name) or an array of strings. |
| `errorTileUrl` | `String`          | `''`    | 要显示的图块图像的URL代替无法加载的图块。                    |
| `zoomOffset`   | `Number`          | `0`     | 磁贴URL中使用的缩放编号将与此值偏移。                        |
| `tms`          | `Boolean`         | `false` | If `true`, inverses Y axis numbering for tiles (turn this on for [TMS](https://en.wikipedia.org/wiki/Tile_Map_Service) services). |
| `zoomReverse`  | `Boolean`         | `false` | 如果设置为true，则在图块网址中使用的缩放倍数将反转（`maxZoom - zoom`而不是`zoom`） |
| `detectRetina` | `Boolean`         | `false` | If `true` and user is on a retina display, it will request four tiles of half the specified size and a bigger zoom level in place of one to utilize the high resolution. |
| `crossOrigin`  | `Boolean`         | `false` | 如果为true，则所有图块的crossOrigin属性都将设置为“”。如果要访问图块像素数据，这是必需的。 |

▶从[GridLayer](https://leafletjs.com/reference-1.2.0.html#gridlayer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[GridLayer](https://leafletjs.com/reference-1.2.0.html#gridlayer)继承的[事件](https://leafletjs.com/reference-1.2.0.html#gridlayer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### Methods

| 方法                                                  | 退货          | 描述                                                         |
| ----------------------------------------------------- | ------------- | ------------------------------------------------------------ |
| `setUrl(<String>*url*, <Boolean>*noRedraw?*)`     | `this`        | 更新图层的URL模板并重绘（除非`noRedraw`设置为`true`）。      |
| `createTile(<Object>*coords*, <Function>*done?*)` | `HTMLElement` | Called only internally, overrides GridLayer's [`createTile()`](https://leafletjs.com/reference-1.2.0.html#gridlayer-createtile) to return an `<img>`HTML element with the appropiate image URL given `coords`. The `done` callback is called when the tile has been loaded. |

#### 扩展方式

扩展层[`TileLayer`](https://leafletjs.com/reference-1.2.0.html#tilelayer)可能会重新实现以下方法。

| 方法                               | 退货     | 描述                                                         |
| ---------------------------------- | -------- | ------------------------------------------------------------ |
| `getTileUrl(<Object>*coords*)` | `String` | 仅在内部调用，返回给定瓷砖坐标的URL。类扩展[`TileLayer`](https://leafletjs.com/reference-1.2.0.html#tilelayer)可以覆盖此功能，以提供自定义图块URL命名方案。 |

▶从[GridLayer](https://leafletjs.com/reference-1.2.0.html#gridlayer)继承的方法

▶ Methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 瓷砖图层

用于将[WMS](https://en.wikipedia.org/wiki/Web_Map_Service)服务显示为地图上的图块层。扩展[`TileLayer`](https://leafletjs.com/reference-1.2.0.html#tilelayer)。

### 使用范例

```
var nexrad = L.tileLayer.wms("http://mesonet.agron.iastate.edu/cgi-bin/wms/nexrad/n0r.cgi", {
    layers: 'nexrad-n0r-900913',
    format: 'image/png',
    transparent: true,
    attribution: "Weather data © 2012 IEM Nexrad"
});
```

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.tileLayer.wms(<String> *baseUrl*,<TileLayer.WMS options> *options*)` | 给定WMS服务的基本URL和WMS参数/选项对象，实例化WMS切片图层对象。 |

### 选件

如果使用此处未记录的任何自定义选项，它们将作为每个请求URL中的额外参数发送到WMS服务器。这对于[非标准供应商WMS参数](http://docs.geoserver.org/stable/en/user/services/wms/vendor.html)很有用 。

| Option            | 类型      | 默认           | 描述                                                         |
| ----------------- | --------- | -------------- | ------------------------------------------------------------ |
| `layers`      | `String`  | `''`           | （必需）要显示的WMS图层的逗号分隔列表。                  |
| `styles`      | `String`  | `''`           | WMS样式的逗号分隔列表。                                      |
| `format`      | `String`  | `'image/jpeg'` | WMS图像格式（`'image/png'`用于具有透明性的图层）。           |
| `transparent` | `Boolean` | `false`        | If `true`, the WMS service will return images with transparency. |
| `version`     | `String`  | `'1.1.1'`      | 要使用的WMS服务的版本                                        |
| `crs`         | `CRS`     | `null`         | 用于WMS请求的坐标参考系统，默认情况下映射CRS。如果不确定是什么意思，请不要更改。 |
| `uppercase`   | `Boolean` | `false`        | If `true`, WMS request parameter keys will be uppercase.     |

▶从[TileLayer](https://leafletjs.com/reference-1.2.0.html#tilelayer)继承的选项

▶从[GridLayer](https://leafletjs.com/reference-1.2.0.html#gridlayer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### Events

▶从[GridLayer](https://leafletjs.com/reference-1.2.0.html#gridlayer)继承的[事件](https://leafletjs.com/reference-1.2.0.html#gridlayer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶ Tooltip events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### 方法

| Method                                                   | 退货   | 描述                                                         |
| -------------------------------------------------------- | ------ | ------------------------------------------------------------ |
| `setParams(<Object> *params*, <Boolean>*noRedraw?*)` | `this` | 合并具有新参数的对象，并在当前屏幕上重新请求磁贴（除非`noRedraw`设置为true）。 |

▶从[TileLayer](https://leafletjs.com/reference-1.2.0.html#tilelayer)继承的方法

▶ Methods inherited from [GridLayer](https://leafletjs.com/reference-1.2.0.html#gridlayer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶ Popup methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 图片叠加层

用于加载和显示地图特定范围内的单个图像。扩展[`Layer`](https://leafletjs.com/reference-1.2.0.html#layer)。

### 使用范例

```
var imageUrl = 'http://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg',
    imageBounds = [[40.712216, -74.22655], [40.773941, -74.12544]];
L.imageOverlay(imageUrl, imageBounds).addTo(map);
```

### 创建

| 厂                                                           | 描述                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| `L.imageOverlay(<String> *imageUrl*, <LatLngBounds> *bounds*,<ImageOverlay options> *options?*)` | 给定图像的URL及其绑定的地理范围，实例化图像覆盖对象。 |

### Options

| 选项                  | 类型      | 默认    | 描述                                                         |
| --------------------- | --------- | ------- | ------------------------------------------------------------ |
| `opacity`         | `Number`  | `1.0`   | 图像叠加层的不透明度。                                       |
| `alt`             | `String`  | `''`    | `alt`图像属性的文本（用于辅助功能）。                        |
| `interactive`     | `Boolean` | `false` | 如果为`true`，则图像叠加层在单击或悬停时将发出[鼠标事件](https://leafletjs.com/reference-1.2.0.html#interactive-layer)。 |
| `crossOrigin`     | `Boolean` | `false` | If true, the image will have its crossOrigin attribute set to ''. This is needed if you want to access image pixel data. |
| `errorOverlayUrl` | `String`  | `''`    | 叠加层图片的网址，用于显示无法加载的叠加层。                 |
| `zIndex`          | `Number`  | `1`     | 平铺层的显式[zIndex](https://developer.mozilla.org/docs/Web/CSS/CSS_Positioning/Understanding_z_index)。 |
| `className`       | `String`  | `''`    | 分配给图像的自定义类名称。默认为空。                         |

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

| 事件         | Data    | 描述                               |
| ------------ | ------- | ---------------------------------- |
| `load `  | `Event` | 当ImageOverlay图层加载其图像时触发 |
| `error ` | `Event` | 当ImageOverlay图层加载其图像时触发 |

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶ Events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶ Tooltip events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### 方法

| 方法                                     | 退货           | 描述                                                         |
| ---------------------------------------- | -------------- | ------------------------------------------------------------ |
| `setOpacity(<Number> *opacity*)`     | `this`         | Sets the opacity of the overlay.                             |
| `bringToFront()`                     | `this`         | 将图层置于所有叠加层的顶部。                                 |
| `bringToBack()`                      | `this`         | 将图层置于所有叠加层的底部。                                 |
| `setUrl(<String> *url*)`             | `this`         | 更改图像的URL。                                              |
| `setBounds(<LatLngBounds> *bounds*)` | `this`         | 更新此ImageOverlay覆盖的边界更改图像叠加层的[zIndex](https://leafletjs.com/reference-1.2.0.html#imageoverlay-zindex)。 |
| `getBounds()`                        | `LatLngBounds` | 获取此ImageOverlay涵盖的范围                                 |
| `getElement()`                       | `HTMLElement`  | 返回[`HTMLImageElement`](https://developer.mozilla.org/docs/Web/API/HTMLImageElement) 此叠加层使用的实例。 |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶ Tooltip methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## VideoOverlay

用于在地图的特定范围内加载和显示视频播放器。扩展[`ImageOverlay`](https://leafletjs.com/reference-1.2.0.html#imageoverlay)。视频叠加层使用[``](https://developer.mozilla.org/docs/Web/HTML/Element/video) HTML5元素。

### 使用范例

```
var videoUrl = 'https://www.mapbox.com/bites/00188/patricia_nasa.webm',
    videoBounds = [[ 32, -130], [ 13, -100]];
L.VideoOverlay(videoUrl, videoBounds ).addTo(map);
```

### Creation

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.videoOverlay(<String|Array|HTMLVideoElement> *video*,<LatLngBounds> *bounds*, <VideoOverlay options> *options?*)` | 给定视频的URL（或URL数组，甚至是视频元素）及其绑定的地理范围，实例化图像覆盖对象。 |

### 选件

| 选项           | 类型      | 默认   | 描述                                                         |
| -------------- | --------- | ------ | ------------------------------------------------------------ |
| `autoplay` | `Boolean` | `true` | 加载后视频是否自动开始播放。                                 |
| `loop`     | `Boolean` | `true` | Whether the video will loop back to the beginning when played. |

▶从[ImageOverlay](https://leafletjs.com/reference-1.2.0.html#imageoverlay)继承的选项

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### Events

| 事件        | 数据    | 描述                   |
| ----------- | ------- | ---------------------- |
| `load ` | `Event` | 视频加载完第一帧后触发 |

▶ Events inherited from [ImageOverlay](https://leafletjs.com/reference-1.2.0.html#imageoverlay)

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法               | 退货               | 描述                                                         |
| ------------------ | ------------------ | ------------------------------------------------------------ |
| `getElement()` | `HTMLVideoElement` | Returns the instance of [`HTMLVideoElement`](https://developer.mozilla.org/docs/Web/API/HTMLVideoElement) used by this overlay. |

▶从[ImageOverlay](https://leafletjs.com/reference-1.2.0.html#imageoverlay)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从继承的弹出方法[Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## Path

一个抽象类，其中包含矢量叠加层（多边形，折线和圆）之间共享的选项和常量。不要直接使用它。扩展[`Layer`](https://leafletjs.com/reference-1.2.0.html#layer)。

### 创建

| 厂                                         | 描述                          |
| ------------------------------------------ | ----------------------------- |
| `L.svg(<Renderer options> *options?*)` | 使用给定的选项创建SVG渲染器。 |

### 选件

| Option                    | 类型       | 默认        | 描述                                                         |
| ------------------------- | ---------- | ----------- | ------------------------------------------------------------ |
| `stroke`              | `Boolean`  | `true`      | 是否沿路径绘制笔划。设置`false`为禁用多边形或圆形上的边框。  |
| `color`               | `String`   | `'#3388ff'` | 笔触颜色                                                     |
| `weight`              | `Number`   | `3`         | Stroke width in pixels                                       |
| `opacity`             | `Number`   | `1.0`       | 脑卒中不透明                                                 |
| `lineCap`             | `String`   | `'round'`   | A string that defines [shape to be used at the end](https://developer.mozilla.org/docs/Web/SVG/Attribute/stroke-linecap) of the stroke. |
| `lineJoin`            | `String`   | `'round'`   | 一个字符串，它定义[要在](https://developer.mozilla.org/docs/Web/SVG/Attribute/stroke-linejoin)笔划[的拐角处使用的形状](https://developer.mozilla.org/docs/Web/SVG/Attribute/stroke-linejoin)。 |
| `dashArray`           | `String`   | `null`      | 定义笔划[破折号图案的](https://developer.mozilla.org/docs/Web/SVG/Attribute/stroke-dasharray)字符串。[`Canvas`](https://leafletjs.com/reference-1.2.0.html#canvas)在[某些旧版浏览器中](https://developer.mozilla.org/docs/Web/API/CanvasRenderingContext2D/setLineDash#Browser_compatibility)无法在支持功能的层上使用。 |
| `dashOffset`          | `String`   | `null`      | A string that defines the [distance into the dash pattern to start the dash](https://developer.mozilla.org/docs/Web/SVG/Attribute/stroke-dashoffset). Doesn't work on [`Canvas`](https://leafletjs.com/reference-1.2.0.html#canvas)-powered layers in [some old browsers](https://developer.mozilla.org/docs/Web/API/CanvasRenderingContext2D/setLineDash#Browser_compatibility). |
| `fill`                | `Boolean`  | `depends`   | 是否用颜色填充路径。设置`false`为禁用多边形或圆形填充。      |
| `fillColor`           | `String`   | `*`         | 填色。默认为[`color`](https://leafletjs.com/reference-1.2.0.html#path-color) option |
| `fillOpacity`         | `Number`   | `0.2`       | 填充不透明度。                                               |
| `fillRule`            | `String`   | `'evenodd'` | 一个字符串，定义[如何](https://developer.mozilla.org/docs/Web/SVG/Attribute/fill-rule)确定[形状的内部](https://developer.mozilla.org/docs/Web/SVG/Attribute/fill-rule)。 |
| `bubblingMouseEvents` | `Boolean`  | `true`      | When `true`, a mouse event on this path will trigger the same event on the map (unless [`L.DomEvent.stopPropagation`](https://leafletjs.com/reference-1.2.0.html#domevent-stoppropagation) is used). |
| `renderer`            | `Renderer` | ``          | 将此特定实例[`Renderer`](https://leafletjs.com/reference-1.2.0.html#renderer)用于此路径。优先于地图的[默认渲染器](https://leafletjs.com/reference-1.2.0.html#map-renderer)。 |
| `className`           | `String`   | `null`      | 在元素上设置的自定义类名称。仅适用于SVG渲染器。              |

▶ Options inherited from [Interactive layer](https://leafletjs.com/reference-1.2.0.html#interactive-layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶ Events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法                                   | 退货   | 描述                                                         |
| -------------------------------------- | ------ | ------------------------------------------------------------ |
| `redraw()`                         | `this` | 重绘图层。更改路径使用的坐标后，有时会很有用。               |
| `setStyle(<Path options> *style*)` | `this` | 根据[`Path options`](https://leafletjs.com/reference-1.2.0.html#path-option)对象中的选项更改路径的外观。 |
| `bringToFront()`                   | `this` | 将图层置于所有路径图层的顶部。                               |
| `bringToBack()`                    | `this` | 将图层置于所有路径图层的底部。                               |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 折线

用于在地图上绘制折线叠加层的类。扩展[`Path`](https://leafletjs.com/reference-1.2.0.html#path)。

### 使用范例

```
// create a red polyline from an array of LatLng points
var latlngs = [
    [45.51, -122.68],
    [37.77, -122.43],
    [34.04, -118.2]
];
var polyline = L.polyline(latlngs, {color: 'red'}).addTo(map);
// zoom the map to the polyline
map.fitBounds(polyline.getBounds());
```

您还可以传递一个多维数组来表示`MultiPolyline`形状：

```
// create a red polyline from an array of arrays of LatLng points
var latlngs = [
    [[45.51, -122.68],
     [37.77, -122.43],
     [34.04, -118.2]],
    [[40.78, -73.91],
     [41.83, -87.62],
     [32.76, -96.72]]
];
```

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.polyline(<LatLng[]> *latlngs*,<Polyline options> *options?*)` | 给定一系列地理点和可选的选项对象，以实例化折线对象。您可以通过传递一系列地理点数组来创建[`Polyline`](https://leafletjs.com/reference-1.2.0.html#polyline)具有多行分隔线（`MultiPolyline`）的对象。 |

### 选件

| 选项               | 类型      | 默认    | 描述                                                         |
| ------------------ | --------- | ------- | ------------------------------------------------------------ |
| `smoothFactor` | `Number`  | `1.0`   | 每个缩放级别简化多义线的程度。更多意味着更好的性能和更平滑的外观，更少意味着更准确的表示。 |
| `noClip`       | `Boolean` | `false` | 禁用折线裁剪。                                               |

▶从[路径](https://leafletjs.com/reference-1.2.0.html#path)继承的选项

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法                                  | 退货           | 描述                                                         |
| ------------------------------------- | -------------- | ------------------------------------------------------------ |
| `toGeoJSON()`                     | `Object`       | 返回一个 [`GeoJSON`](https://leafletjs.com/reference-1.2.0.html#geojson)折线的表示形式（作为GeoJSON `LineString`或`MultiLineString`要素）。 |
| `getLatLngs()`                    | `LatLng[]`     | 返回路径中的点数组，如果是多段线，则返回嵌套的点数组。       |
| `setLatLngs(<LatLng[]>*latlngs*)` | `this`         | 用给定的地理点数组替换折线中的所有点。                       |
| `isEmpty()`                       | `Boolean`      | Returns `true` if the Polyline has no LatLngs.               |
| `getCenter()`                     | `LatLng`       | 返回折线的中心（[质心](http://en.wikipedia.org/wiki/Centroid)）。 |
| `getBounds()`                     | `LatLngBounds` | Returns the [`LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds) of the path. |
| `addLatLng(<LatLng>*latlng*)`     | `this`         | 将指定点添加到折线。默认情况下，如果是多段折线，则会添加到多段线的第一个环，但可以通过将特定的环作为LatLng数组传递来覆盖（您可以使用进行更早的访问[`getLatLngs`](https://leafletjs.com/reference-1.2.0.html#polyline-getlatlngs)）。 |

▶从[Path](https://leafletjs.com/reference-1.2.0.html#path)继承的方法

▶ Methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶ Methods inherited from [Evented](https://leafletjs.com/reference-1.2.0.html#evented)

## 多边形

用于在地图上绘制多边形叠加层的类。扩展[`Polyline`](https://leafletjs.com/reference-1.2.0.html#polyline)。请注意，创建多边形时传递的点不应具有等于第一个点的附加最后一个点-最好过滤掉这些点。

### 使用范例

```
// create a red polygon from an array of LatLng points
var latlngs = [[37, -109.05],[41, -109.03],[41, -102.05],[37, -102.04]];
var polygon = L.polygon(latlngs, {color: 'red'}).addTo(map);
// zoom the map to the polygon
map.fitBounds(polygon.getBounds());
```

您还可以传递由latlng组成的数组的数组，其中第一个数组代表外形，其他数组代表外形中的孔：

```
var latlngs = [
  [[37, -109.05],[41, -109.03],[41, -102.05],[37, -102.04]], // outer ring
  [[37.29, -108.58],[40.71, -108.58],[40.71, -102.50],[37.29, -102.50]] // hole
];
```

此外，您可以传递一个多维数组来表示MultiPolygon形状。

```
var latlngs = [
  [ // first polygon
    [[37, -109.05],[41, -109.03],[41, -102.05],[37, -102.04]], // outer ring
    [[37.29, -108.58],[40.71, -108.58],[40.71, -102.50],[37.29, -102.50]] // hole
  ],
  [ // second polygon
    [[41, -111.03],[45, -111.04],[45, -104.05],[41, -104.05]]
  ]
];
```

### 创建

| Factory                                                      | 描述 |
| ------------------------------------------------------------ | ---- |
| `L.polygon(<LatLng[]> *latlngs*, <Polyline options> *options?*)` |      |

### 选件

▶从[折线](https://leafletjs.com/reference-1.2.0.html#polyline)继承的选项

▶从[路径](https://leafletjs.com/reference-1.2.0.html#path)继承的选项

▶ Options inherited from [Interactive layer](https://leafletjs.com/reference-1.2.0.html#interactive-layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶ Popup events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法              | 退货     | 描述                                                         |
| ----------------- | -------- | ------------------------------------------------------------ |
| `toGeoJSON()` | `Object` | Returns a [`GeoJSON`](https://leafletjs.com/reference-1.2.0.html#geojson)多边形的表示形式（作为GeoJSON [`Polygon`](https://leafletjs.com/reference-1.2.0.html#polygon)或`MultiPolygon`要素）。 |

▶从[折线](https://leafletjs.com/reference-1.2.0.html#polyline)继承的方法

▶从[Path](https://leafletjs.com/reference-1.2.0.html#path)继承的方法

▶ Methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶ Methods inherited from [Evented](https://leafletjs.com/reference-1.2.0.html#evented)

## 长方形

用于在地图上绘制矩形叠加层的类。扩展[`Polygon`](https://leafletjs.com/reference-1.2.0.html#polygon)。

### 使用范例

```
// define rectangle geographical bounds
var bounds = [[54.559322, -5.767822], [56.1210604, -3.021240]];
// create an orange rectangle
L.rectangle(bounds, {color: "#ff7800", weight: 1}).addTo(map);
// zoom the map to the rectangle bounds
map.fitBounds(bounds);
```

### 创建

| 厂                                                           | 描述 |
| ------------------------------------------------------------ | ---- |
| `L.rectangle(<LatLngBounds> *latLngBounds*, <Polyline options> *options?*)` |      |

### Options

▶从[折线](https://leafletjs.com/reference-1.2.0.html#polyline)继承的选项

▶从[路径](https://leafletjs.com/reference-1.2.0.html#path)继承的选项

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的选项

▶ Options inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### 事件

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶ Popup events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法                                           | 退货   | 描述                                          |
| ---------------------------------------------- | ------ | --------------------------------------------- |
| `setBounds(<LatLngBounds> *latLngBounds*)` | `this` | Redraws the rectangle with the passed bounds. |

▶从[多边形](https://leafletjs.com/reference-1.2.0.html#polygon)继承的方法

▶从[折线](https://leafletjs.com/reference-1.2.0.html#polyline)继承的方法

▶从[Path](https://leafletjs.com/reference-1.2.0.html#path)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶ Popup methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 圈

用于在地图上绘制圆形叠加层的类。扩展[`CircleMarker`](https://leafletjs.com/reference-1.2.0.html#circlemarker)。这是一个近似值，并开始从更接近极点的实圆发散（由于投影变形）。

### 使用范例

```
L.circle([50.5, 30.5], {radius: 200}).addTo(map);
```

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.circle(<LatLng> *latlng*, <Circle options> *options?*)` | 实例化给定地理位置的圆对象和包含圆半径的选项对象。           |
| `L.circle(<LatLng> *latlng*, <Number> *radius*,<Circle options> *options?*)` | 实例化圆的过时方式，以与0.7.x代码兼容。不要在新的应用程序或插件中使用。 |

### 选件

| 选项         | 类型     | 默认 | Description            |
| ------------ | -------- | ---- | ---------------------- |
| `radius` | `Number` | ``   | 圆的半径，以米为单位。 |

▶从[路径](https://leafletjs.com/reference-1.2.0.html#path)继承的选项

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### Methods

| 方法                               | 退货           | 描述                                                         |
| ---------------------------------- | -------------- | ------------------------------------------------------------ |
| `setRadius(<Number> *radius*)` | `this`         | 设置圆的半径。单位为米。                                     |
| `getRadius()`                  | `Number`       | 返回圆的当前半径。单位为米。                                 |
| `getBounds()`                  | `LatLngBounds` | Returns the [`LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds) of the path. |

▶从[CircleMarker](https://leafletjs.com/reference-1.2.0.html#circlemarker)继承的方法

▶从[Path](https://leafletjs.com/reference-1.2.0.html#path)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## CircleMarker

固定大小的圆，半径以像素为单位指定。扩展[`Path`](https://leafletjs.com/reference-1.2.0.html#path)。

### 创建

| 厂                                                           | 描述                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| `L.circleMarker(<LatLng> *latlng*, <CircleMarker options>*options?*)` | 实例化给定地理位置的圆形标记对象和可选的options对象。 |

### 选件

| 选项         | 类型     | 默认 | 描述                         |
| ------------ | -------- | ---- | ---------------------------- |
| `radius` | `Number` | `10` | 圆形标记的半径，以像素为单位 |

▶ Options inherited from [Path](https://leafletjs.com/reference-1.2.0.html#path)

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[交互层](https://leafletjs.com/reference-1.2.0.html#interactive-layer)继承的鼠标事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| Method                             | 退货     | 描述                                                         |
| ---------------------------------- | -------- | ------------------------------------------------------------ |
| `toGeoJSON()`                  | `Object` | 返回一个 [`GeoJSON`](https://leafletjs.com/reference-1.2.0.html#geojson)圆形标记的表示形式（作为GeoJSON [`Point`](https://leafletjs.com/reference-1.2.0.html#point)功能）。 |
| `setLatLng(<LatLng> *latLng*)` | `this`   | 将圆形标记的位置设置为新位置。                               |
| `getLatLng()`                  | `LatLng` | 返回圆形标记的当前地理位置                                   |
| `setRadius(<Number> *radius*)` | `this`   | Sets the radius of a circle marker. Units are in pixels.     |
| `getRadius()`                  | `Number` | 返回圆的当前半径                                             |

▶从[Path](https://leafletjs.com/reference-1.2.0.html#path)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## SVG

尽管SVG在IE7和IE8上不可用，但这些浏览器支持[VML](https://en.wikipedia.org/wiki/Vector_Markup_Language)，在这种情况下，SVG渲染器将退回到VML。VML在2012年已弃用，这意味着存在VML功能仅是为了与Internet Explorer的旧版本向后兼容。

允许使用[SVG](https://developer.mozilla.org/docs/Web/SVG)显示矢量层。继承[`Renderer`](https://leafletjs.com/reference-1.2.0.html#renderer)。由于[技术限制](http://caniuse.com/#search=svg)，SVG并非在所有Web浏览器中都可用，尤其是Android 2.x和3.x。尽管SVG在IE7和IE8上不可用，但这些浏览器支持 [VML](https://en.wikipedia.org/wiki/Vector_Markup_Language) （现已弃用的技术），在这种情况下，SVG渲染器将退回到VML。

### Usage example

默认情况下，将SVG用于地图中的所有路径：

```
var map = L.map('map', {
    renderer: L.svg()
});
```

将SVG渲染器与额外的填充一起用于特定的矢量几何：

```
var map = L.map('map');
var myRenderer = L.svg({ padding: 0.5 });
var line = L.polyline( coordinates, { renderer: myRenderer } );
var circle = L.circle( center, { renderer: myRenderer } );
```

### 选件

▶从[渲染器](https://leafletjs.com/reference-1.2.0.html#renderer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[Renderer](https://leafletjs.com/reference-1.2.0.html#renderer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

### 功能

在不实例化L.SVG的情况下可以调用几个静态函数：

| Function                                                 | 退货         | 描述                                                         |
| -------------------------------------------------------- | ------------ | ------------------------------------------------------------ |
| `create(<String> *name*)`                            | `SVGElement` | 返回[SVGElement](https://developer.mozilla.org/docs/Web/API/SVGElement)的实例，该实例与传递的类名相对应。例如，使用'line'将返回[SVGLineElement](https://developer.mozilla.org/docs/Web/API/SVGLineElement)的实例。 |
| `pointsToPath(<Point[]> *rings*,<Boolean> *closed*)` | `String`     | 生成多个环的SVG路径字符串，每个环都变成“ M..L..L ..”指令     |

## 帆布

允许使用来显示矢量图层[``](https://developer.mozilla.org/docs/Web/API/Canvas_API)。继承[`Renderer`](https://leafletjs.com/reference-1.2.0.html#renderer)。由于[技术限制](http://caniuse.com/#search=canvas)，Canvas并非在所有Web浏览器中都可用，尤其是IE8，并且在某些情况下重叠的几何图形可能无法正确显示。

### 使用范例

默认情况下，将Canvas用于地图中的所有路径：

```
var map = L.map('map', {
    renderer: L.canvas()
});
```

对特定的矢量几何体使用带有额外填充的Canvas渲染器：

```
var map = L.map('map');
var myRenderer = L.canvas({ padding: 0.5 });
var line = L.polyline( coordinates, { renderer: myRenderer } );
var circle = L.circle( center, { renderer: myRenderer } );
```

### 创建

| 厂                                            | 描述                                 |
| --------------------------------------------- | ------------------------------------ |
| `L.canvas(<Renderer options> *options?*)` | 使用给定的选项创建一个Canvas渲染器。 |

### 选件

▶从[渲染器](https://leafletjs.com/reference-1.2.0.html#renderer)继承的选项

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[Renderer](https://leafletjs.com/reference-1.2.0.html#renderer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### Methods

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 图层组

Used to group several layers and handle them as one. If you add it to the map, any layers added or removed from the group will be added/removed on the map as well. Extends [`Layer`](https://leafletjs.com/reference-1.2.0.html#layer).

### 使用范例

```
L.layerGroup([marker1, marker2])
    .addLayer(polyline)
    .addTo(map);
```

### 创建

| 厂                                      | 描述                                       |
| --------------------------------------- | ------------------------------------------ |
| `L.layerGroup(<Layer[]> *layers?*)` | 创建一个图层组，可以选择指定一组初始图层。 |

### 选件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### Events

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法                                                 | 退货      | 描述                                                         |
| ---------------------------------------------------- | --------- | ------------------------------------------------------------ |
| `toGeoJSON()`                                    | `Object`  | 返回一个 [`GeoJSON`](https://leafletjs.com/reference-1.2.0.html#geojson)层组的表示（作为以GeoJSON `FeatureCollection`，`GeometryCollection`或`MultiPoint`）。 |
| `addLayer(<Layer> *layer*)`                      | `this`    | 将给定的图层添加到组中。                                     |
| `removeLayer(<Layer> *layer*)`                   | `this`    | 从组中删除给定的层。                                         |
| `removeLayer(<Number> *id*)`                     | `this`    | 从组中删除具有给定内部ID的图层。                             |
| `hasLayer(<Layer> *layer*)`                      | `Boolean` | Returns `true` if the given layer is currently added to the group. |
| `hasLayer(<Number> *id*)`                        | `Boolean` | 返回当前`true`是否将给定的内部ID添加到该组。                 |
| `clearLayers()`                                  | `this`    | 从组中删除所有图层。                                         |
| `invoke(<String> *methodName*,*…*)`              | `this`    | 调用`methodName`此组中包含的每个层，并传递任何其他参数。如果所包含的图层未实现，则无效`methodName`。 |
| `eachLayer(<Function> *fn*,<Object> *context?*)` | `this`    | 在组的各个层上进行迭代，可以选择指定迭代器函数的上下文。`group.eachLayer(function (layer) {     layer.bindPopup('Hello'); }); ` |
| `getLayer(<Number> *id*)`                        | `Layer`   | 返回具有给定内部ID的图层。                                   |
| `getLayers()`                                    | `Layer[]` | 返回添加到组中的所有图层的数组。                             |
| `setZIndex(<Number> *zIndex*)`                   | `this`    | 调用`setZIndex`此组中包含的每个图层，并传递z-index。         |
| `getLayerId(<Layer> *layer*)`                    | `Number`  | 返回图层的内部ID                                             |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 功能组

扩展后[`LayerGroup`](https://leafletjs.com/reference-1.2.0.html#layergroup)，可以更轻松地对其所有成员层执行相同的操作：

- [`bindPopup`](https://leafletjs.com/reference-1.2.0.html#layer-bindpopup)一次将弹出窗口绑定到所有层（同样使用[`bindTooltip`](https://leafletjs.com/reference-1.2.0.html#layer-bindtooltip)）
- 事件会传播到[`FeatureGroup`](https://leafletjs.com/reference-1.2.0.html#featuregroup)，因此，如果该组具有事件处理程序，它将处理来自任何层的事件。这包括鼠标事件和自定义事件。
- 有`layeradd`和`layerremove`事件

### 使用范例

```
L.featureGroup([marker1, marker2, polyline])
    .bindPopup('Hello world!')
    .on('click', function() { alert('Clicked on a member of the group!'); })
    .addTo(map);
```

### 创建

| 厂                                       | 描述                                       |
| ---------------------------------------- | ------------------------------------------ |
| `L.featureGroup(<Layer[]> *layers*)` | 创建一个要素组，可以选择提供一组初始图层。 |

### 选件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

| 事件               | 数据         | 描述                                                         |
| ------------------ | ------------ | ------------------------------------------------------------ |
| `layeradd `    | `LayerEvent` | 向其中添加图层时触发 [`FeatureGroup`](https://leafletjs.com/reference-1.2.0.html#featuregroup) |
| `layerremove ` | `LayerEvent` | 从中移除图层时触发 [`FeatureGroup`](https://leafletjs.com/reference-1.2.0.html#featuregroup) |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法                                   | 退货           | 描述                                                     |
| -------------------------------------- | -------------- | -------------------------------------------------------- |
| `setStyle(<Path options> *style*)` | `this`         | 将给定的路径选项设置为具有`setStyle`方法的组的每一层。   |
| `bringToFront()`                   | `this`         | 将图层组置于所有其他图层的顶部                           |
| `bringToBack()`                    | `this`         | Brings the layer group to the back of all other layers   |
| `getBounds()`                      | `LatLngBounds` | 返回要素组的LatLngBounds（根据其子级的边界和坐标创建）。 |

▶从[LayerGroup](https://leafletjs.com/reference-1.2.0.html#layergroup)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## GeoJSON

表示一个GeoJSON对象或一个GeoJSON对象数组。允许您解析GeoJSON数据并将其显示在地图上。扩展[`FeatureGroup`](https://leafletjs.com/reference-1.2.0.html#featuregroup)。

### 使用范例

```
L.geoJSON(data, {
    style: function (feature) {
        return {color: feature.properties.color};
    }
}).bindPopup(function (layer) {
    return layer.feature.properties.description;
}).addTo(map);
```

### 创建

| 厂                                                           | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.geoJSON(<Object> *geojson?*,<GeoJSON options> *options?*)` | 创建一个GeoJSON图层。（可选）接受[GeoJSON格式](https://tools.ietf.org/html/rfc7946)的对象 以显示在地图上（您可以稍后使用`addData`方法将其添加）和一个`options`对象。 |

### 选件

| 选项                 | 类型       | 默认 | 描述                                                         |
| -------------------- | ---------- | ---- | ------------------------------------------------------------ |
| `pointToLayer`   | `Function` | `*`  | 一个`Function`定义了如何以GeoJSON点产卵单张层。在添加数据时，会在内部调用它，并传递GeoJSON点特征及其[`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)。默认是生成默认值[`Marker`](https://leafletjs.com/reference-1.2.0.html#marker)：`function(geoJsonPoint, latlng) {     return L.marker(latlng); } ` |
| `style`          | `Function` | `*`  | 用于`Function`定义[`Path options`](https://leafletjs.com/reference-1.2.0.html#path-option)GeoJSON线和面的样式的，在添加数据时在内部调用。默认值是不覆盖任何默认值：`function (geoJsonFeature) {     return {} } ` |
| `onEachFeature`  | `Function` | `*`  | 一个`Function`将被调用一次，每个创建`Feature`已创建和风格后，。用于将事件和弹出窗口附加到功能部件。默认是不对新创建的图层执行任何操作：`function (feature, layer) {} ` |
| `filter`         | `Function` | `*`  | 一个`Function`将被用于决定是否包括特征或没有。默认值为包括所有功能：`function (geoJsonFeature) {     return true; } `注意：动态更改`filter`选项仅对新添加的数据有效。它*不会*重新评估已经包含的功能。 |
| `coordsToLatLng` | `Function` | `*`  | 一个`Function`将被用于转换GeoJSON的坐标[`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)秒。默认为`coordsToLatLng`静态方法。 |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

▶从[FeatureGroup](https://leafletjs.com/reference-1.2.0.html#featuregroup)继承的[事件](https://leafletjs.com/reference-1.2.0.html#featuregroup)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法                      | 退货   | 描述                                                         |
| ------------------------- | ------ | ------------------------------------------------------------ |
| `addData(*data*)`     | `this` | 将GeoJSON对象添加到图层。                                    |
| `resetStyle(*layer*)` | `this` | 将给定的矢量层样式重置为原始的GeoJSON样式，这对于在悬停事件之后重置样式很有用。 |
| `setStyle(*style*)`   | `this` | 使用给定的样式函数更改GeoJSON矢量图层的样式。                |

▶从[FeatureGroup](https://leafletjs.com/reference-1.2.0.html#featuregroup)继承的方法

▶从[LayerGroup](https://leafletjs.com/reference-1.2.0.html#layergroup)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

### 功能

有几个静态函数可以在不实例化L.GeoJSON的情况下调用：

| 功能                                                         | 退货     | 描述                                                         |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| `geometryToLayer(<Object>*featureData*, <GeoJSON options>*options?*)` | `Layer`  | 创建一个 [`Layer`](https://leafletjs.com/reference-1.2.0.html#layer) from a given GeoJSON feature. Can use a custom [`pointToLayer`](https://leafletjs.com/reference-1.2.0.html#geojson-pointtolayer) and/or [`coordsToLatLng`](https://leafletjs.com/reference-1.2.0.html#geojson-coordstolatlng) functions if provided as options. |
| `coordsToLatLng(<Array> *coords*)`                       | `LatLng` | [`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)根据在GeoJSON中用于点的2个数字（经度，纬度）或3个数字（经度，纬度，高度）的数组创建对象。 |
| `coordsToLatLngs(<Array> *coords*,<Number> *levelsDeep?*, <Function>*coordsToLatLng?*)` | `Array`  | [`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)根据GeoJSON坐标数组创建的多维数组。 `levelsDeep`指定嵌套级别（0表示点数组，1表示点数组，依此类推，默认为0）。可以使用自定义[`coordsToLatLng`](https://leafletjs.com/reference-1.2.0.html#geojson-coordstolatlng)功能。 |
| `latLngToCoords(<LatLng> *latlng*,<Number> *precision?*)` | `Array`  | 反向 [`coordsToLatLng`](https://leafletjs.com/reference-1.2.0.html#geojson-coordstolatlng) |
| `latLngsToCoords(<Array> *latlngs*,<Number> *levelsDeep?*, <Boolean>*closed?*)` | `Array`  | Reverse of [`coordsToLatLngs`](https://leafletjs.com/reference-1.2.0.html#geojson-coordstolatlngs) `closed` determines whether the first point should be appended to the end of the array to close the feature, only used when `levelsDeep` is 0. False by default. |
| `asFeature(<Object> *geojson*)`                          | `Object` | 将GeoJSON几何/功能标准化为GeoJSON功能。                      |

## 网格层

用于处理HTML元素的平铺网格的通用类。这是所有图块层的基本类，并取代`TileLayer.Canvas`。GridLayer可以扩展到创建HTML元素的像平铺格`<canvas>`，`<img>`或`<div>`。GridLayer将为您处理这些DOM元素的创建和动画处理。

### 使用范例

#### Synchronous usage

要创建自定义层，延长GridLayer和实施`createTile()`方法，这将传递一个[`Point`](https://leafletjs.com/reference-1.2.0.html#point)与对象`x`，`y`和`z`（缩放级别）坐标绘制的瓷砖。

```
var CanvasLayer = L.GridLayer.extend({
    createTile: function(coords){
        // create a <canvas> element for drawing
        var tile = L.DomUtil.create('canvas', 'leaflet-tile');
        // setup tile width and height according to the options
        var size = this.getTileSize();
        tile.width = size.x;
        tile.height = size.y;
        // get a canvas context and draw something on it using coords.x, coords.y and coords.z
        var ctx = tile.getContext('2d');
        // return the tile so it can be rendered on screen
        return tile;
    }
});
```

#### 异步使用

磁贴创建也可以是异步的，这在使用第三方图形库时非常有用。磁贴完成绘制后，可以将其传递给`done()`回调。

```
var CanvasLayer = L.GridLayer.extend({
    createTile: function(coords, done){
        var error;
        // create a <canvas> element for drawing
        var tile = L.DomUtil.create('canvas', 'leaflet-tile');
        // setup tile width and height according to the options
        var size = this.getTileSize();
        tile.width = size.x;
        tile.height = size.y;
        // draw something asynchronously and pass the tile to the done() callback
        setTimeout(function() {
            done(error, tile);
        }, 1000);
        return tile;
    }
});
```

### 创建

| 厂                                                | 描述                                  |
| ------------------------------------------------- | ------------------------------------- |
| `L.gridLayer(<GridLayer options> *options?*)` | 使用提供的选项创建GridLayer的新实例。 |

### 选件

| 选项                    | 类型           | 默认         | 描述                                                         |
| ----------------------- | -------------- | ------------ | ------------------------------------------------------------ |
| `tileSize`          | `Number|Point` | `256`        | 网格中图块的宽度和高度。如果宽度和高度相等，则使用数字，`L.point(width, height)`否则使用数字。 |
| `opacity`           | `Number`       | `1.0`        | 瓷砖的不透明度。可以在`createTile()`功能中使用。             |
| `updateWhenIdle`    | `Boolean`      | `(depends)`  | 仅在平移结束时加载新的图块。 `true`在移动浏览器上，默认情况下是为了避免过多的请求并保持流畅的导航。 `false`否则，为了*在*平移*期间*显示新的图块，因为[`keepBuffer`](https://leafletjs.com/reference-1.2.0.html#gridlayer-keepbuffer)在桌面浏览器中轻松平移该选项之外 。 |
| `updateWhenZooming` | `Boolean`      | `true`       | 默认情况下，平滑缩放动画（在[触摸缩放](https://leafletjs.com/reference-1.2.0.html#map-touchzoom)或期间[`flyTo()`](https://leafletjs.com/reference-1.2.0.html#map-flyto)）将在每个整数缩放级别更新网格层。将此选项设置为`false`仅在平滑动画结束时才会更新网格层。 |
| `updateInterval`    | `Number`       | `200`        | `updateInterval`平移时，平铺块每毫秒更新一次不会超过一次。   |
| `zIndex`            | `Number`       | `1`          | 平铺层的显式zIndex。                                         |
| `bounds`            | `LatLngBounds` | `undefined`  | If set, tiles will only be loaded inside the set [`LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds). |
| `minZoom`           | `Number`       | `0`          | 显示该层的最小缩放级别（含）。                               |
| `maxZoom`           | `Number`       | `undefined`  | 将显示此图层的最大缩放级别（含）。                           |
| `maxNativeZoom`     | `Number`       | `undefined`  | 磁贴源可用的最大缩放数。如果指定，则所有缩放级别高于且`maxNativeZoom`将从`maxNativeZoom`级别加载并自动缩放的图块。 |
| `minNativeZoom`     | `Number`       | `undefined`  | Minimum zoom number the tile source has available. If it is specified, the tiles on all zoom levels lower than `minNativeZoom`will be loaded from `minNativeZoom` level and auto-scaled. |
| `noWrap`            | `Boolean`      | `false`      | 图层是否环绕在子午线周围。如果为`true`，则GridLayer仅在低缩放级别下显示一次。当地[图CRS](https://leafletjs.com/reference-1.2.0.html#map-crs)不环绕时不起作用。可以与[`bounds`](https://leafletjs.com/reference-1.2.0.html#bounds) 以防止请求超出CRS限制的图块。 |
| `pane`              | `String`       | `'tilePane'` | `Map pane` 将在其中添加网格层的位置。                        |
| `className`         | `String`       | `''`         | 分配给图块层的自定义类名称。默认为空。                       |
| `keepBuffer`        | `Number`       | `2`          | When panning the map, keep this many rows and columns of tiles before unloading them. |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

| 事件                 | 数据             | 描述                                     |
| -------------------- | ---------------- | ---------------------------------------- |
| `loading `       | `Event`          | 当网格图层开始加载图块时触发。           |
| `tileunload `    | `TileEvent`      | 移除磁贴时触发（例如，磁贴离开屏幕时）。 |
| `tileloadstart ` | `TileEvent`      | 当请求图块并开始加载时触发。             |
| `tileerror `     | `TileErrorEvent` | 加载图块发生错误时触发。                 |
| `tileload `      | `TileEvent`      | 瓷砖加载时触发。                         |
| `load `          | `Event`          | 当网格图层加载所有可见的图块时触发。     |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶ Popup events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

| 方法                                | 退货          | 描述                                                         |
| ----------------------------------- | ------------- | ------------------------------------------------------------ |
| `bringToFront()`                | `this`        | 将图块层置于所有图块层的顶部。                               |
| `bringToBack()`                 | `this`        | 将图块层置于所有图块层的底部。                               |
| `getContainer()`                | `HTMLElement` | Returns the HTML element that contains the tiles for this layer. |
| `setOpacity(<Number>*opacity*)` | `this`        | 更改网格层的[不透明度](https://leafletjs.com/reference-1.2.0.html#gridlayer-opacity)。 |
| `setZIndex(<Number> *zIndex*)`  | `this`        | 更改网格图层的[zIndex](https://leafletjs.com/reference-1.2.0.html#gridlayer-zindex)。 |
| `isLoading()`                   | `Boolean`     | 返回`true`如果在网格层有块尚未加载完毕。                     |
| `redraw()`                      | `this`        | 使图层清除所有图块并再次请求它们。                           |
| `getTileSize()`                 | `Point`       | Normalizes the [tileSize option](https://leafletjs.com/reference-1.2.0.html#gridlayer-tilesize) into a point. Used by the `createTile()`method. |

#### 扩展方式

扩展层[`GridLayer`](https://leafletjs.com/reference-1.2.0.html#gridlayer)应重新实现以下方法。

| 方法                                                  | 退货          | 描述                                                         |
| ----------------------------------------------------- | ------------- | ------------------------------------------------------------ |
| `createTile(<Object>*coords*, <Function>*done?*)` | `HTMLElement` | 仅在内部调用，必须由extensions类覆盖[`GridLayer`](https://leafletjs.com/reference-1.2.0.html#gridlayer)。返回`HTMLElement`对应于给定的`coords`。如果`done`指定了回调，则必须在磁贴完成加载和绘制后调用它。 |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶ Popup methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 纬度

代表具有一定纬度和经度的地理位置。

### 使用范例

```
var latlng = L.latLng(50.5, 30.5);
```

所有接受LatLng对象的Leaflet方法也以简单的Array形式和简单的对象形式（除非另有说明）接受它们，因此这些行是等效的：

```
map.panTo([50, 30]);
map.panTo({lon: 30, lat: 50});
map.panTo({lat: 50, lng: 30});
map.panTo(L.latLng(50, 30));
```

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.latLng(<Number> *latitude*, <Number>*longitude*, <Number> *altitude?*)` | Creates an object representing a geographical point with the given latitude and longitude (and optionally altitude). |
| `L.latLng(<Array> *coords*)`                             | 需要一个形式为`[Number, Number]`或的数组`[Number, Number, Number]`。 |
| `L.latLng(<Object> *coords*)`                            | 期望格式为纯对象`{lat: Number, lng: Number}`或`{lat: Number, lng: Number, alt: Number}`替代。 |

### 方法

| 方法                                                       | 退货           | 描述                                                         |
| ---------------------------------------------------------- | -------------- | ------------------------------------------------------------ |
| `equals(<LatLng> *otherLatLng*,<Number> *maxMargin?*)` | `Boolean`      | 返回`true`如果给定的[`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)点是在相同的位置（错误的小幅度的范围内）。可以通过设置`maxMargin`较小的数字来覆盖误差范围。 |
| `toString()`                                           | `String`       | Returns a string representation of the point (for debugging purposes). |
| `distanceTo(<LatLng>*otherLatLng*)`                    | `Number`       | 返回[`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)使用[Haversine公式](http://en.wikipedia.org/wiki/Haversine_formula)计算的给定距离（以米为单位）。 |
| `wrap()`                                               | `LatLng`       | 返回一个新的[`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)对象，其中包含经度，因此它始终在-180到+180度之间。 |
| `toBounds(<Number>*sizeInMeters*)`                     | `LatLngBounds` | 返回一个新[`LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds)对象，其中每个边界与都`sizeInMeters/2`相距米[`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)。 |

### 性质

| 属性       | 类型     | 描述             |
| ---------- | -------- | ---------------- |
| `lat ` | `Number` | 纬度（度）       |
| `lng ` | `Number` | 经度（度）       |
| `alt ` | `Number` | 海拔高度（可选） |

## LatLngBounds

表示地图上的矩形地理区域。

### 使用范例

```
var corner1 = L.latLng(40.712, -74.227),
corner2 = L.latLng(40.774, -74.125),
bounds = L.latLngBounds(corner1, corner2);
```

All Leaflet methods that accept LatLngBounds objects also accept them in a simple Array form (unless noted otherwise), so the bounds example above can be passed like this:

```
map.fitBounds([
    [40.712, -74.227],
    [40.774, -74.125]
]);
```

注意：如果该区域跨越了一个子午线（通常与国际日期变更线相混淆），则必须指定经度-[-180，180]度范围*之外*的角。

### 创建

| 厂                                                          | 描述                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| `L.latLngBounds(<LatLng> *corner1*,<LatLng> *corner2*)` | [`LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds)通过定义矩形的两个对角相对的角来创建对象。 |
| `L.latLngBounds(<LatLng[]> *latlngs*)`                  | 创建一个[`LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds)由其包含的地理位置定义的对象。对于将地图缩放到适合特定位置的位置非常有用[`fitBounds`](https://leafletjs.com/reference-1.2.0.html#map-fitbounds)。 |

### 方法

| 方法                                                         | 退货           | Description                                                  |
| ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| `extend(<LatLng> *latlng*)`                              | `this`         | 扩展范围以包含给定点                                         |
| `extend(<LatLngBounds>*otherBounds*)`                    | `this`         | 扩展范围以包含给定范围                                       |
| `pad(<Number> *bufferRatio*)`                            | `LatLngBounds` | 返回通过在每个方向上将当前范围扩展给定百分比而创建的更大范围。 |
| `getCenter()`                                            | `LatLng`       | 返回边界的中心点。                                           |
| `getSouthWest()`                                         | `LatLng`       | 返回边界的西南点。                                           |
| `getNorthEast()`                                         | `LatLng`       | 返回边界的东北点。                                           |
| `getNorthWest()`                                         | `LatLng`       | 返回边界的西北点。                                           |
| `getSouthEast()`                                         | `LatLng`       | 返回边界的东南点。                                           |
| `getWest()`                                              | `Number`       | 返回范围的西经                                               |
| `getSouth()`                                             | `Number`       | 返回边界的南纬                                               |
| `getEast()`                                              | `Number`       | 返回边界的东经                                               |
| `getNorth()`                                             | `Number`       | Returns the north latitude of the bounds                     |
| `contains(<LatLngBounds>*otherBounds*)`                  | `Boolean`      | 返回`true`矩形是否包含给定的矩形。                           |
| `contains(<LatLng> *latlng*)`                            | `Boolean`      | 返回`true`矩形是否包含给定点。                               |
| `intersects(<LatLngBounds>*otherBounds*)`                | `Boolean`      | 返回`true`矩形是否与给定边界相交。如果两个边界至少有一个共同点，则它们相交。 |
| `overlaps(<Bounds>*otherBounds*)`                        | `Boolean`      | 返回`true`矩形是否与给定边界重叠。如果两个边界的交点是一个区域，则两个边界重叠。 |
| `toBBoxString()`                                         | `String`       | 以'southwest_lng，southwest_lat，northeast_lng，northeast_lat'格式返回带有边界框坐标的字符串。用于将请求发送到返回地理数据的Web服务。 |
| `equals(<LatLngBounds>*otherBounds*, <Number>*maxMargin?*)` | `Boolean`      | 返回`true`该矩形是否等效于给定范围（在很小的误差范围内）。可以通过设置`maxMargin`较小的数字来覆盖误差范围。 |
| `isValid()`                                              | `Boolean`      | 返回`true`边界是否已正确初始化。                             |

## 点

用`x`和表示`y`像素点。

### 使用范例

```
var point = L.point(200, 300);
```

所有接受[`Point`](https://leafletjs.com/reference-1.2.0.html#point)对象的Leaflet方法和选项也都以简单的Array形式接受它们（除非另有说明），因此这些行是等效的：

```
map.panBy([200, 300]);
map.panBy(L.point(200, 300));
```

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.point(<Number> *x*, <Number> *y*,<Boolean> *round?*)` | Creates a Point object with the given `x` and `y` coordinates. If optional `round` is set to true, rounds the `x` and `y` values. |
| `L.point(<Number[]> *coords*)`                           | 需要一个表单数组`[x, y]`。                                   |
| `L.point(<Object> *coords*)`                             | 期望使用表单的普通对象`{x: Number, y: Number}`。             |

### 方法

| 方法                                  | 退货      | 描述                                                         |
| ------------------------------------- | --------- | ------------------------------------------------------------ |
| `clone()`                         | `Point`   | 返回当前点的副本。                                           |
| `add(<Point> *otherPoint*)`       | `Point`   | 返回当前点和给定点相加的结果。                               |
| `subtract(<Point>*otherPoint*)`   | `Point`   | 返回从电流中减去给定点的结果。                               |
| `divideBy(<Number> *num*)`        | `Point`   | 返回当前点除以给定数字的结果。                               |
| `multiplyBy(<Number> *num*)`      | `Point`   | 返回当前点与给定数字相乘的结果。                             |
| `scaleBy(<Point> *scale*)`        | `Point`   | 将当前点的每个坐标乘以的每个坐标 `scale`。用线性代数术语，将点乘以 定义的[缩放矩阵](https://en.wikipedia.org/wiki/Scaling_%28geometry%29#Matrix_representation)`scale`。 |
| `unscaleBy(<Point> *scale*)`      | `Point`   | 的逆`scaleBy`。将当前点的每个坐标除以的每个坐标`scale`。     |
| `round()`                         | `Point`   | 返回带有舍入坐标的当前点的副本。                             |
| `floor()`                         | `Point`   | 返回具有底坐标（向下舍入）的当前点的副本。                   |
| `ceil()`                          | `Point`   | 返回带有最高坐标（向上舍入）的当前点的副本。                 |
| `distanceTo(<Point>*otherPoint*)` | `Number`  | 返回当前点和给定点之间的笛卡尔距离。                         |
| `equals(<Point>*otherPoint*)`     | `Boolean` | `true`如果给定点具有相同的坐标，则返回。                     |
| `contains(<Point>*otherPoint*)`   | `Boolean` | 返回`true`给定的点的两个坐标是小于相应的当前点坐标（绝对值）。 |
| `toString()`                      | `String`  | 返回该点的字符串表示形式，以进行调试。                       |

### 性质

| 属性     | 类型     | 描述        |
| -------- | -------- | ----------- |
| `x ` | `Number` | `x`点的坐标 |
| `y ` | `Number` | `y`点的坐标 |

## 界线

用像素坐标表示一个矩形区域。

### Usage example

```
var p1 = L.point(10, 10),
p2 = L.point(40, 60),
bounds = L.bounds(p1, p2);
```

所有接受[`Bounds`](https://leafletjs.com/reference-1.2.0.html#bounds)对象的Leaflet方法也以简单的Array形式接受它们（除非另有说明），因此上面的bounds示例可以这样传递：

```
otherBounds.intersects([[10, 10], [40, 60]]);
```

### 创建

| 厂                                                   | 描述                               |
| ---------------------------------------------------- | ---------------------------------- |
| `L.bounds(<Point> *corner1*, <Point> *corner2*)` | 从两个角坐标对创建一个Bounds对象。 |
| `L.bounds(<Point[]> *points*)`                   | 从给定的点数组创建一个Bounds对象。 |

### 方法

| 方法                                    | 退货      | 描述                                                         |
| --------------------------------------- | --------- | ------------------------------------------------------------ |
| `extend(<Point> *point*)`           | `this`    | Extends the bounds to contain the given point.               |
| `getCenter(<Boolean> *round?*)`     | `Point`   | 返回边界的中心点。                                           |
| `getBottomLeft()`                   | `Point`   | 返回边界的左下角。                                           |
| `getTopRight()`                     | `Point`   | 返回边界的右上角。                                           |
| `getTopLeft()`                      | `Point`   | 返回边界的左上角点（即[`this.min`](https://leafletjs.com/reference-1.2.0.html#bounds-min)）。 |
| `getBottomRight()`                  | `Point`   | 返回边界的右下角（即[`this.max`](https://leafletjs.com/reference-1.2.0.html#bounds-max)）。 |
| `getSize()`                         | `Point`   | Returns the size of the given bounds                         |
| `contains(<Bounds>*otherBounds*)`   | `Boolean` | 返回`true`矩形是否包含给定的矩形。                           |
| `contains(<Point> *point*)`         | `Boolean` | 返回`true`矩形是否包含给定点。                               |
| `intersects(<Bounds>*otherBounds*)` | `Boolean` | 返回`true`矩形是否与给定边界相交。如果两个边界至少有一个共同点，则它们相交。 |
| `overlaps(<Bounds>*otherBounds*)`   | `Boolean` | 返回`true`矩形是否与给定边界重叠。如果两个边界的交点是一个区域，则两个边界重叠。 |

### 性质

| Property   | 类型    | 描述           |
| ---------- | ------- | -------------- |
| `min ` | `Point` | 矩形的左上角。 |
| `max ` | `Point` | 矩形的右下角。 |

## 图标

表示创建标记时要提供的图标。

### 使用范例

```
var myIcon = L.icon({
    iconUrl: 'my-icon.png',
    iconSize: [38, 95],
    iconAnchor: [22, 94],
    popupAnchor: [-3, -76],
    shadowUrl: 'my-icon-shadow.png',
    shadowSize: [68, 95],
    shadowAnchor: [22, 94]
});
L.marker([50.505, 30.57], {icon: myIcon}).addTo(map);
```

[`L.Icon.Default`](https://leafletjs.com/reference-1.2.0.html#icon-default)扩展[`L.Icon`](https://leafletjs.com/reference-1.2.0.html#icon)，是Leaflet默认用于标记的蓝色图标。

### 创建

| 厂                                     | 描述                             |
| -------------------------------------- | -------------------------------- |
| `L.icon(<Icon options> *options*)` | 使用给定的选项创建一个图标实例。 |

### 选件

| 选项                  | 类型     | 默认   | 描述                                                         |
| --------------------- | -------- | ------ | ------------------------------------------------------------ |
| `iconUrl`         | `String` | `null` | （必需）图标图像的URL（绝对或相对于脚本路径）。          |
| `iconRetinaUrl`   | `String` | `null` | 视网膜大小的图标图像版本的URL（绝对或相对于脚本路径）。用于视网膜屏幕设备。 |
| `iconSize`        | `Point`  | `null` | 图标图像的大小（以像素为单位）。                             |
| `iconAnchor`      | `Point`  | `null` | 图标“尖端”的坐标（相对于其左上角）。图标将对齐，以便此点位于标记的地理位置。如果指定了大小，则默认为居中，也可以在CSS中设置负边距。 |
| `popupAnchor`     | `Point`  | `null` | 相对于图标锚点，弹出窗口将“打开”的点的坐标。                 |
| `shadowUrl`       | `String` | `null` | 图标阴影图像的URL。如果未指定，则不会创建阴影图像。          |
| `shadowRetinaUrl` | `String` | `null` |                                                              |
| `shadowSize`      | `Point`  | `null` | 阴影图像的大小（以像素为单位）。                             |
| `shadowAnchor`    | `Point`  | `null` | 阴影“尖端”的坐标（相对于其左上角）（与iconAnchor相同，如果未指定）。 |
| `className`       | `String` | `''`   | 既要分配给图标图像也要分配给阴影图像的自定义类名称。默认为空。 |

### 方法

| 方法                                        | 退货          | 描述                                                         |
| ------------------------------------------- | ------------- | ------------------------------------------------------------ |
| `createIcon(<HTMLElement>*oldIcon?*)`   | `HTMLElement` | Called internally when the icon has to be shown, returns a `<img>` HTML element styled according to the options. |
| `createShadow(<HTMLElement>*oldIcon?*)` | `HTMLElement` | 为`createIcon`，但为它下面的阴影。                           |

### 图标默认

一个普通的子类[`Icon`](https://leafletjs.com/reference-1.2.0.html#icon)，表示[`Marker`](https://leafletjs.com/reference-1.2.0.html#marker)未指定图标时在s中使用的图标。指向随传单发行的蓝色标记图像。为了自定义默认图标，只需更改的属性`L.Icon.Default.prototype.options` （一组[`Icon options`](https://leafletjs.com/reference-1.2.0.html#icon-option)）。如果要*完全*替换默认图标，`L.Marker.prototype.options.icon`请改用您自己的图标。

| 选项            | 类型     | 默认 | 描述                                                         |
| --------------- | -------- | ---- | ------------------------------------------------------------ |
| `imagePath` | `String` | ``   | [`Icon.Default`](https://leafletjs.com/reference-1.2.0.html#icon-default)将尝试自动检测蓝色图标图像的绝对位置。如果您以非标准方式放置这些图像，请将此选项设置为指向正确的绝对路径。 |

## DivIcon

表示使用简单`<div>` 元素而不是图像的标记的轻量级图标。继承自，[`Icon`](https://leafletjs.com/reference-1.2.0.html#icon)但忽略`iconUrl`和阴影选项。

### 使用范例

```
var myIcon = L.divIcon({className: 'my-div-icon'});
// you can set .my-div-icon styles in CSS
L.marker([50.505, 30.57], {icon: myIcon}).addTo(map);
```

默认情况下，它具有一个“ leaflet-div-icon” CSS类，其样式为带有阴影的白色小方块。

### 创建

| 厂                                           | 描述                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| `L.divIcon(<DivIcon options> *options*)` | [`DivIcon`](https://leafletjs.com/reference-1.2.0.html#divicon)使用给定的选项创建一个实例。 |

### 选件

| 选项        | 类型     | 默认     | 描述                                                    |
| ----------- | -------- | -------- | ------------------------------------------------------- |
| `html`  | `String` | `''`     | 将自定义HTML代码放入div元素内，默认情况下为空。         |
| `bgPos` | `Point`  | `[0, 0]` | Optional relative position of the background, in pixels |

▶从[图标](https://leafletjs.com/reference-1.2.0.html#icon)继承的选项

### 方法

▶从[Icon](https://leafletjs.com/reference-1.2.0.html#icon)继承的方法

## 控制缩放

一个基本的缩放控件，带有两个按钮（放大和缩小）。除非您将其[`zoomControl`选项](https://leafletjs.com/reference-1.2.0.html#map-zoomcontrol)设置为，否则默认情况下会将其放置在地图上`false`。扩展[`Control`](https://leafletjs.com/reference-1.2.0.html#control)。

### 创建

| 厂                                                     | 描述             |
| ------------------------------------------------------ | ---------------- |
| `L.control.zoom(<Control.Zoom options> *options*)` | 创建一个缩放控件 |

### 选件

| Option             | 类型     | 默认         | 描述                        |
| ------------------ | -------- | ------------ | --------------------------- |
| `zoomInText`   | `String` | `'+'`        | 在“放大”按钮上设置的文字。  |
| `zoomInTitle`  | `String` | `'Zoom in'`  | 标题设置在“放大”按钮上。    |
| `zoomOutText`  | `String` | `'&#x2212`   | '在“缩小”按钮上设置的文本。 |
| `zoomOutTitle` | `String` | `'Zoom out'` | 在“缩小”按钮上设置的标题。  |

▶从[控制](https://leafletjs.com/reference-1.2.0.html#control)继承的选项

### Methods

▶从[Control](https://leafletjs.com/reference-1.2.0.html#control)继承的方法

## 控制归因

归因控件可让您在地图上的小文本框中显示归因数据。除非您将其[`attributionControl`选项](https://leafletjs.com/reference-1.2.0.html#map-attributioncontrol)设置为`false`，否则默认情况下会将其放置在地图上，并且它会使用[`getAttribution`方法](https://leafletjs.com/reference-1.2.0.html#layer-getattribution)自动从图层中获取属性文字。扩展控制。

### 创建

| 厂                                                           | 描述               |
| ------------------------------------------------------------ | ------------------ |
| `L.control.attribution(<Control.Attribution options> *options*)` | 创建一个归因控件。 |

### 选件

| 选项         | 类型     | 默认        | 描述                                                         |
| ------------ | -------- | ----------- | ------------------------------------------------------------ |
| `prefix` | `String` | `'Leaflet'` | The HTML text shown before the attributions. Pass `false` to disable. |

▶从[控制](https://leafletjs.com/reference-1.2.0.html#control)继承的选项

### 方法

| 方法                                     | 退货   | 描述                                                |
| ---------------------------------------- | ------ | --------------------------------------------------- |
| `setPrefix(<String> *prefix*)`       | `this` | 在属性之前设置文字。                                |
| `addAttribution(<String> *text*)`    | `this` | 添加归因文本（例如`'Vector data &copy; Mapbox'`）。 |
| `removeAttribution(<String> *text*)` | `this` | 删除归因文本。                                      |

▶ Methods inherited from [Control](https://leafletjs.com/reference-1.2.0.html#control)

## 控制层

图层控件使用户能够在不同的基础层之间切换以及打开/关闭覆盖（请查看[详细示例](https://leafletjs.com/examples/layers-control/)）。扩展[`Control`](https://leafletjs.com/reference-1.2.0.html#control)。

### 使用范例

```
var baseLayers = {
    "Mapbox": mapbox,
    "OpenStreetMap": osm
};
var overlays = {
    "Marker": marker,
    "Roads": roadsLayer
};
L.control.layers(baseLayers, overlays).addTo(map);
```

的`baseLayers`和`overlays`参数是对象常量与层名作为密钥和[`Layer`](https://leafletjs.com/reference-1.2.0.html#layer)对象作为值：

```
{
    "<someName1>": layer1,
    "<someName2>": layer2
}
```

图层名称可以包含HTML，该HTML允许您向项目添加其他样式：

```
{"<img src='my-layer-icon' /> <span class='my-layer-item'>My Layer</span>": myLayer}
```

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.control.layers(<Object>*baselayers?*, <Object> *overlays?*,<Control.Layers options> *options?*)` | 使用给定的图层创建归因控件。基本层将通过单选按钮进行切换，而覆盖层将通过复选框进行切换。请注意，所有基础层都应在基础层对象中传递，但在地图实例化期间仅应将一个基础层添加到地图中。 |

### 选件

| 选项                 | 类型       | Default | 描述                                                         |
| -------------------- | ---------- | ------- | ------------------------------------------------------------ |
| `collapsed`      | `Boolean`  | `true`  | 如果为`true`，则控件将折叠为一个图标，并在鼠标悬停或触摸时展开。 |
| `autoZIndex`     | `Boolean`  | `true`  | 如果为`true`，则控件将按递增顺序将zIndexes分配给其所有图层，以便在打开/关闭zIndex时保留顺序。 |
| `hideSingleBase` | `Boolean`  | `false` | 如果为`true`，则只有一层时，控件中的基础层将被隐藏。         |
| `sortLayers`     | `Boolean`  | `false` | 是否对图层排序。`false`设为时，图层将保持添加到控件的顺序。  |
| `sortFunction`   | `Function` | `*`     | 甲[比较功能](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) 将被用于排序所述层，当`sortLayers`是`true`。该函数同时接收[`L.Layer`](https://leafletjs.com/reference-1.2.0.html#layer)实例及其名称，如中所示 `sortFunction(layerA, layerB, nameA, nameB)`。默认情况下，它按名称的字母顺序对图层进行排序。 |

▶ Options inherited from [Control](https://leafletjs.com/reference-1.2.0.html#control)

### 方法

| 方法                                                 | 退货   | 描述                                             |
| ---------------------------------------------------- | ------ | ------------------------------------------------ |
| `addBaseLayer(<Layer> *layer*, <String> *name*)` | `this` | 将具有给定名称的基础层（单选按钮项）添加到控件。 |
| `addOverlay(<Layer> *layer*, <String> *name*)`   | `this` | 将具有给定名称的叠加层（复选框条目）添加到控件。 |
| `removeLayer(<Layer> *layer*)`                   | `this` | 从控件中删除给定的层。                           |
| `expand()`                                       | `this` | 如果折叠，则展开控制容器。                       |
| `collapse()`                                     | `this` | Collapse the control container if expanded.      |

▶从[Control](https://leafletjs.com/reference-1.2.0.html#control)继承的方法

## 控制规模

一个简单的比例尺控件，以公制（m / km）和英制（mi / ft）系统显示当前屏幕中心的比例。扩展[`Control`](https://leafletjs.com/reference-1.2.0.html#control)。

### 使用范例

```
L.control.scale().addTo(map);
```

### 创建

| 厂                                                        | 描述                         |
| --------------------------------------------------------- | ---------------------------- |
| `L.control.scale(<Control.Scale options> *options?*)` | 使用给定的选项创建比例控件。 |

### 选件

| 选项                 | 类型      | 默认    | 描述                                                         |
| -------------------- | --------- | ------- | ------------------------------------------------------------ |
| `maxWidth`       | `Number`  | `100`   | Maximum width of the control in pixels. The width is set dynamically to show round values (e.g. 100, 200, 500). |
| `metric`         | `Boolean` | `True`  | 是否显示公制刻度线（米/公里）。                              |
| `imperial`       | `Boolean` | `True`  | 是否显示英制刻度线（mi / ft）。                              |
| `updateWhenIdle` | `Boolean` | `false` | 如果为`true`，则控件在上更新[`moveend`](https://leafletjs.com/reference-1.2.0.html#map-moveend)，否则，它始终是最新的（在上更新[`move`](https://leafletjs.com/reference-1.2.0.html#map-move)）。 |

▶从[控制](https://leafletjs.com/reference-1.2.0.html#control)继承的选项

### 方法

▶从[Control](https://leafletjs.com/reference-1.2.0.html#control)继承的方法

## 浏览器

Leaflet内部使用的具有静态属性的名称空间，用于浏览器/功能检测。

### 使用范例

```
if (L.Browser.ielt9) {
  alert('Upgrade your browser, dude!');
}
```

### 性质

| 属性                 | 类型      | 描述                                                         |
| -------------------- | --------- | ------------------------------------------------------------ |
| `ie `            | `Boolean` | `true` 适用于所有Internet Explorer版本（不是Edge）。         |
| `ielt9 `         | `Boolean` | `true` 低于9的Internet Explorer版本。                        |
| `edge `          | `Boolean` | `true` 适用于Edge Web浏览器。                                |
| `webkit `        | `Boolean` | `true` 适用于基于Webkit的浏览器，例如Chrome和Safari（包括移动版本）。 |
| `android `       | `Boolean` | `true` 适用于在Android平台上运行的任何浏览器。               |
| `android23 `     | `Boolean` | `true` 适用于在Android 2或Android 3上运行的浏览器。          |
| `opera `         | `Boolean` | `true` 用于Opera浏览器                                       |
| `chrome `        | `Boolean` | `true` 适用于Chrome浏览器。                                  |
| `gecko `         | `Boolean` | `true` 适用于基于壁虎的浏览器，例如Firefox。                 |
| `safari `        | `Boolean` | `true` 用于Safari浏览器。                                    |
| `opera12 `       | `Boolean` | `true` 适用于支持CSS转换（版本12或更高版本）的Opera浏览器。  |
| `win `           | `Boolean` | `true` 当浏览器在Windows平台上运行时                         |
| `ie3d `          | `Boolean` | `true` 适用于所有支持CSS转换的Internet Explorer版本。        |
| `webkit3d `      | `Boolean` | `true` 适用于支持CSS转换的基于Webkit的浏览器。               |
| `gecko3d `       | `Boolean` | `true` 适用于支持CSS转换的基于壁虎的浏览器。                 |
| `any3d `         | `Boolean` | `true` 适用于所有支持CSS转换的浏览器。                       |
| `mobile `        | `Boolean` | `true` for all browsers running in a mobile device.          |
| `mobileWebkit `  | `Boolean` | `true` 适用于移动设备中所有基于Webkit的浏览器。              |
| `mobileWebkit3d` | `Boolean` | `true` 适用于支持CSS转换的移动设备中的所有基于Webkit的浏览器。 |
| `msPointer `     | `Boolean` | `true` 适用于实施Microsoft触摸事件模型（尤其是IE10）的浏览器。 |
| `pointer `       | `Boolean` | `true`适用于所有支持[指针事件的](https://msdn.microsoft.com/en-us/library/dn433244%28v=vs.85%29.aspx)浏览器。 |
| `touch `         | `Boolean` | `true`适用于所有支持[触摸事件的](https://developer.mozilla.org/docs/Web/API/Touch_events)浏览器。这并不一定意味着浏览器在带有触摸屏的计算机上运行，仅意味着浏览器能够理解触摸事件。 |
| `mobileOpera `   | `Boolean` | `true` 适用于移动设备中的Opera浏览器。                       |
| `mobileGecko `   | `Boolean` | `true` 适用于在移动设备上运行的基于壁虎的浏览器。            |
| `retina `        | `Boolean` | `true` 适用于高分辨率“视网膜”屏幕上的浏览器。                |
| `canvas `        | `Boolean` | `true`当浏览器支持时[``](https://developer.mozilla.org/docs/Web/API/Canvas_API)。 |
| `svg `           | `Boolean` | `true`当浏览器支持[SVG时](https://developer.mozilla.org/docs/Web/SVG)。 |
| `vml `           | `Boolean` | `true`如果浏览器支持[VML](https://en.wikipedia.org/wiki/Vector_Markup_Language)。 |

## 实用程序

Leaflet内部使用的各种实用程序功能。

### 功能

| 功能                                                         | 退货        | 描述                                                         |
| ------------------------------------------------------------ | ----------- | ------------------------------------------------------------ |
| `extend(<Object> *dest*,<Object> *src?*)`                | `Object`    | 将`src`对象（或多个对象）的属性合并为`dest`对象并返回后者。有`L.extend`捷径。 |
| `create(<Object> *proto*,<Object> *properties?*)`        | `Object`    | [Object.create的](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object/create)兼容性polyfill |
| `bind(<Function> *fn*, *…*)`                             | `Function`  | Returns a new function bound to the arguments passed, like [Function.prototype.bind](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Function/bind). Has a `L.bind()` shortcut. |
| `stamp(<Object> *obj*)`                                  | `Number`    | 返回对象的唯一ID，如果没有，则辅助它。                       |
| `throttle(<Function> *fn*,<Number> *time*, <Object>*context*)` | `Function`  | 返回一个函数，该函数`fn`在给定范围内执行函数`context` （以便`this`关键字引用`context`inside `fn`的代码）。`fn`每给定数量的，该函数 将被调用不超过一次`time`。绑定函数收到的参数将是绑定函数时传递的任何参数，然后是调用绑定函数时传递的任何参数。有`L.throttle`捷径。 |
| `wrapNum(<Number> *num*,<Number[]> *range*, <Boolean>*includeMax?*)` | `Number`    | 以这样的方式返回`num`模数`range`，使其位于`range[0]`and 内 `range[1]`。返回值将始终小于 `range[1]`除非`includeMax`设置为`true`。 |
| `falseFn()`                                              | `Function`  | 返回始终返回的函数`false`。                                  |
| `formatNum(<Number> *num*,<Number> *digits?*)`           | `Number`    | Returns the number `num` rounded to `digits` decimals, or to 5 decimals by default. |
| `trim(<String> *str*)`                                   | `String`    | [String.prototype.trim的](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)兼容性[polyfill](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String/Trim) |
| `splitWords(<String> *str*)`                             | `String[]`  | 修剪并在空白处分割字符串，然后返回部分数组。                 |
| `setOptions(<Object> *obj*,<Object> *options*)`          | `Object`    | 合并给定的属性添加到`options`了的`obj`对象，返回结果的选项。请参阅`Class options`。有`L.setOptions`捷径。 |
| `getParamString(<Object>*obj*, <String> *existingUrl?*,<Boolean> *uppercase?*)` | `String`    | 将对象转换为参数URL字符串，例如，`{a: "foo", b: "bar"}` 转换为`'?a=foo&b=bar'`。如果`existingUrl`设置为，则参数将附加在末尾。如果`uppercase`是`true``'?A=foo&B=bar'` |
| `template(<String> *str*,<Object> *data*)`               | `String`    | 简单的模板工具，接受格式的模板字符串`'Hello {a}, {b}'` 和类似的数据对象`{a: 'foo', b: 'bar'}`，返回已评估的string `('Hello foo, bar')`。您还可以为数据值指定函数而不是字符串-它们将通过传递来评估`data` |
| `isArray(*obj*)`                                         | `Boolean`   | 兼容的polyfill                                               |
| `indexOf(<Array> *array*,<Object> *el*)`                 | `Number`    | Compatibility polyfill for                                   |
| `requestAnimFrame(<Function>*fn*, <Object> *context?*,<Boolean> *immediate?*)` | `Number`    | 计划`fn`在浏览器重新绘制时执行。`fn`势必 `context`如果给。当`immediate`设置，`fn`立即调用如果浏览器不具备原生支持 [`window.requestAnimationFrame`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) |
| `cancelAnimFrame(<Number>*id*)`                          | `undefined` | 取消上一个`requestAnimFrame`。另请参见[window.cancelAnimationFrame](https://developer.mozilla.org/docs/Web/API/window/cancelAnimationFrame)。 |

### 性质

| 属性                | 类型     | 描述                                                         |
| ------------------- | -------- | ------------------------------------------------------------ |
| `lastId `       | `Number` | 上次使用的唯一ID [`stamp()`](https://leafletjs.com/reference-1.2.0.html#util-stamp) |
| `emptyImageUrl` | `String` | 数据URI字符串，包含以base64编码的空GIF图像。用作破解程序，以从基于WebKit的移动设备上未使用的图像中释放内存（通过将图像设置`src`为此字符串）。 |

## Transformation

代表仿射变换：一组系数`a`，`b`，`c`，`d` 转化形式的点`(x, y)`进`(a*x + b, c*y + d)`，做相反。Leaflet在其投影代码中使用。

### 使用范例

```
var transformation = L.transformation(2, 5, -1, 10),
    p = L.point(1, 2),
    p2 = transformation.transform(p), //  L.point(7, 8)
    p3 = transformation.untransform(p2); //  L.point(1, 2)
```

### 创建

| 厂                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.transformation(<Number> *a*, <Number> *b*, <Number>*c*, <Number> *d*)` | 使用给定的系数实例化一个Transformation对象。                 |
| `L.transformation(<Array> *coefficients*)`               | 需要一个形式的系数数组 `[a: Number, b: Number, c: Number, d: Number]`。 |

### 方法

| 方法                                                 | 退货    | 描述                                                         |
| ---------------------------------------------------- | ------- | ------------------------------------------------------------ |
| `transform(<Point> *point*,<Number> *scale?*)`   | `Point` | Returns a transformed point, optionally multiplied by the given scale. Only accepts actual [`L.Point`](https://leafletjs.com/reference-1.2.0.html#point) instances, not arrays. |
| `untransform(<Point> *point*,<Number> *scale?*)` | `Point` | 返回给定点的逆变换，可以选择除以给定比例。仅接受实际[`L.Point`](https://leafletjs.com/reference-1.2.0.html#point)实例，不接受数组。 |

## LineUtil

Leafine内部使用各种实用程序功能来处理多点线，以使折线快如闪电。

### 功能

| 功能                                                         | 退货              | 描述                                                         |
| ------------------------------------------------------------ | ----------------- | ------------------------------------------------------------ |
| `simplify(<Point[]> *points*,<Number> *tolerance*)`      | `Point[]`         | 使用[Douglas-Peucker算法](http://en.wikipedia.org/wiki/Douglas-Peucker_algorithm)，在保留折线形状的同时显着减少了折线的数量，并返回了新的简化点数组 。当处理/显示每个缩放级别的Leaflet折线时，可大大提高性能，并减少视觉噪音。容差会影响简化的程度（值越小意味着质量越高，但速度越慢且分数越高）。也作为单独的微库[Simplify.js发布](http://mourner.github.com/simplify-js/)。 |
| `pointToSegmentDistance(<Point>*p*, <Point> *p1*, <Point> *p2*)` | `Number`          | 将点`p`和线段之间的距离返回`p1`到`p2`。                      |
| `closestPointOnSegment(<Point>*p*, <Point> *p1*, <Point> *p2*)` | `Number`          | Returns the closest point from a point `p` on a segment `p1` to `p2`. |
| `clipSegment(<Point> *a*, <Point>*b*, <Bounds> *bounds*, <Boolean>*useLastCode?*, <Boolean>*round?*)` | `Point[]|Boolean` | 使用[Cohen-Sutherland算法](https://en.wikipedia.org/wiki/Cohen%E2%80%93Sutherland_algorithm)通过矩形边界将段a剪切为b （直接修改段点！）。Leaflet用于仅显示屏幕上或附近的折线点，从而提高了性能。 |
| `isFlat(<LatLng[]> *latlngs*)`                           | `Boolean`         | 如果`latlngs`为平面数组，则返回true ；嵌套为false。          |

## 多用途

多边形几何的各种实用功能。

### 功能

| 功能                                                         | 退货      | 描述                                                         |
| ------------------------------------------------------------ | --------- | ------------------------------------------------------------ |
| `clipPolygon(<Point[]>*points*, <Bounds> *bounds*,<Boolean> *round?*)` | `Point[]` | 裁剪由给`points`定边界（使用[Sutherland-Hodgeman算法](https://en.wikipedia.org/wiki/Sutherland%E2%80%93Hodgman_algorithm)）由给定定义的多边形几何。Leaflet用于仅显示屏幕上或附近的多边形点，从而提高了性能。请注意，多边形点需要与折线不同的裁剪算法，因此有一种单独的方法。 |

## DomEvent

实用程序功能，可与Leaflet内部使用的[DOM事件一起](https://developer.mozilla.org/docs/Web/API/Event)使用。

### 功能

| 功能                                                         | 退货     | Description                                                  |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| `on(<HTMLElement> *el*, <String> *types*,<Function> *fn*, <Object> *context?*)` | `this`   | 将侦听器函数（`fn`）添加到元素的特定DOM事件类型`el`。您可以选择指定侦听器的上下文（`this`关键字将指向的对象）。您还可以传递几种以空格分隔的类型（例如`'click dblclick'`）。 |
| `on(<HTMLElement> *el*, <Object>*eventMap*, <Object> *context?*)` | `this`   | 添加一组类型/侦听器对，例如 `{click: onClick, mousemove: onMouseMove}` |
| `off(<HTMLElement> *el*, <String> *types*,<Function> *fn*, <Object> *context?*)` | `this`   | 删除以前添加的侦听器功能。如果未指定函数，它将从元素中删除该特定DOM事件的所有侦听器。请注意，如果将自定义上下文传递给on，则必须将相同的上下文传递给`off`，以便删除侦听器。 |
| `off(<HTMLElement> *el*, <Object>*eventMap*, <Object> *context?*)` | `this`   | 删除一组类型/侦听器对，例如 `{click: onClick, mousemove: onMouseMove}` |
| `off(<HTMLElement> *el*)`                                | `this`   | 删除所有已知的事件侦听器                                     |
| `stopPropagation(<DOMEvent> *ev*)`                       | `this`   | 停止给定事件传播到父元素。在侦听器函数内部使用：`L.DomEvent.on(div, 'click', function (ev) {     L.DomEvent.stopPropagation(ev); }); ` |
| `disableScrollPropagation(<HTMLElement>*el*)`            | `this`   | 添加`stopPropagation`到元素的`'mousewheel'`事件（以及浏览器变体）。 |
| `disableClickPropagation(<HTMLElement>*el*)`             | `this`   | Adds `stopPropagation` to the element's `'click'`, `'doubleclick'`, `'mousedown'` and `'touchstart'` events (plus browser variants). |
| `preventDefault(<DOMEvent> *ev*)`                        | `this`   | 防止`ev`发生DOM事件的默认操作（例如，在a元素的href中跟随链接，或在`<form>`提交a时执行带有页面重新加载的POST请求）。在侦听器函数中使用它。 |
| `stop(*ev*)`                                             | `this`   | 是否`stopPropagation`和`preventDefault`在同一时间。          |
| `getMousePosition(<DOMEvent> *ev*,<HTMLElement> *container?*)` | `Point`  | `container`如果未指定，则从DOM事件获取相对于或整个页面的标准化鼠标位置 。 |
| `getWheelDelta(<DOMEvent> *ev*)`                         | `Number` | 从鼠标滚轮DOM事件获取标准化滚轮增量，以滚动的垂直像素为单位（向下滚动时为负）。来自指针设备的事件如果没有精确滚动，则映射到60像素的最佳猜测。 |
| `addListener(*…*)`                                       | `this`   | 别名 [`L.DomEvent.on`](https://leafletjs.com/reference-1.2.0.html#domevent-on) |
| `removeListener(*…*)`                                    | `this`   | 别名 [`L.DomEvent.off`](https://leafletjs.com/reference-1.2.0.html#domevent-off) |

## DomUtil

实用程序功能可与 Leaflet内部使用的[DOM](https://developer.mozilla.org/docs/Web/API/Document_Object_Model)树配合使用。大多数期望或返回a的函数`HTMLElement`也适用于SVG元素。唯一的区别是，类引用HTML中的CSS类和SVG中的SVG类。

### 功能

| 功能                                                         | 退货           | Description                                                  |
| ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| `get(<String|HTMLElement> *id*)`                         | `HTMLElement`  | 返回具有给定元素DOM ID的元素，如果直接传递则返回元素本身。   |
| `getStyle(<HTMLElement> *el*,<String> *styleAttrib*)`    | `String`       | 返回元素上某个样式属性的值，包括计算值或通过CSS设置的值。    |
| `create(<String> *tagName*,<String> *className?*,<HTMLElement> *container?*)` | `HTMLElement`  | 使用创建一个HTML元素`tagName`，将其类设置为`className`，然后选择将其附加到`container`element上。 |
| `remove(<HTMLElement> *el*)`                             | ``             | `el`从其父元素中删除                                         |
| `empty(<HTMLElement> *el*)`                              | ``             | `el`从中删除所有的子元素`el`                                 |
| `toFront(<HTMLElement> *el*)`                            | ``             | 制作`el`其父级的最后一个子级，以便在其他子级之前进行渲染。   |
| `toBack(<HTMLElement> *el*)`                             | ``             | 创建`el`其父级的第一个孩子，因此在其他子项之后进行渲染。     |
| `hasClass(<HTMLElement> *el*,<String> *name*)`           | `Boolean`      | Returns `true` if the element's class attribute contains `name`. |
| `addClass(<HTMLElement> *el*,<String> *name*)`           | ``             | 添加`name`到元素的class属性。                                |
| `removeClass(<HTMLElement>*el*, <String> *name*)`        | ``             | `name`从元素的class属性中移除。                              |
| `setClass(<HTMLElement> *el*,<String> *name*)`           | ``             | 设置元素的类。                                               |
| `getClass(<HTMLElement> *el*)`                           | `String`       | 返回元素的类。                                               |
| `setOpacity(<HTMLElement> *el*,<Number> *opacity*)`      | ``             | 设置元素的不透明度（包括旧的IE支持）。 `opacity`必须是从`0`到的数字`1`。 |
| `testProp(<String[]> *props*)`                           | `String|false` | 遍历样式名称数组，并返回第一个名称，该名称是元素的有效样式名称。如果找不到这样的名称，则返回false。适用于供应商前缀的样式，例如`transform`。 |
| `setTransform(<HTMLElement>*el*, <Point> *offset*, <Number>*scale?*)` | ``             | 重置的3D CSS变换，`el`使其按`offset`像素进行翻译，并可以选择按比例缩放`scale`。如果浏览器不支持3D CSS转换，则无效。 |
| `setPosition(<HTMLElement>*el*, <Point> *position*)`     | ``             | 使用CSS转换或顶部/左侧位置（取决于浏览器）将的位置设置为所`el`指定的坐标`position`（由Leaflet内部用于定位其图层）。 |
| `getPosition(<HTMLElement>*el*)`                         | `Point`        | 返回先前使用setPosition定位的元素的坐标。                    |
| `disableTextSelection()`                                 | ``             | Prevents the user from generating `selectstart` DOM events, usually generated when the user drags the mouse through a page with text. Used internally by Leaflet to override the behaviour of any click-and-drag interaction on the map. Affects drag interactions on the whole document. |
| `enableTextSelection()`                                  | ``             | 取消上一个的效果[`L.DomUtil.disableTextSelection`](https://leafletjs.com/reference-1.2.0.html#domutil-disabletextselection)。 |
| `disableImageDrag()`                                     | ``             | 为[`L.DomUtil.disableTextSelection`](https://leafletjs.com/reference-1.2.0.html#domutil-disabletextselection)，但对于`dragstart`DOM事件，通常在用户拖动图像时生成。 |
| `enableImageDrag()`                                      | ``             | 取消上一个的效果[`L.DomUtil.disableImageDrag`](https://leafletjs.com/reference-1.2.0.html#domutil-disabletextselection)。 |
| `preventOutline(<HTMLElement>*el*)`                      | ``             | 使 元素的[轮廓](https://developer.mozilla.org/docs/Web/CSS/outline)`el`不可见。由Leaflet内部使用，以防止当用户在元素上执行拖动交互时可聚焦的元素显示轮廓。 |
| `restoreOutline()`                                       | ``             | 取消上一个的效果[`L.DomUtil.preventOutline`](https://leafletjs.com/reference-1.2.0.html)。 |

### 性质

| 属性                  | 类型     | 描述                                                         |
| --------------------- | -------- | ------------------------------------------------------------ |
| `TRANSFORM `      | `String` | 供应商前缀的转换样式名称（例如，`'webkitTransform'`对于WebKit）。 |
| `TRANSITION `     | `String` | 供应商前缀的过渡样式名称。                                   |
| `TRANSITION_END ` | `String` | 供应商前缀的transitionend事件名称。                          |

## PosAnimation

内部用于平移动画，利用现代浏览器的CSS3 Transitions和IE6-9的计时器后备功能。

### 使用范例

```
var fx = new L.PosAnimation();
fx.run(el, [300, 500], 0.5);
```

### 建设者

| 建设者                 | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| `L.PosAnimation()` | 创建一个[`PosAnimation`](https://leafletjs.com/reference-1.2.0.html#posanimation)对象。 |

### 事件

| 事件         | 数据    | 描述                   |
| ------------ | ------- | ---------------------- |
| `start ` | `Event` | 动画开始时触发         |
| `step `  | `Event` | 在动画过程中连续发射。 |
| `end `   | `Event` | 动画结束时触发。       |

### 方法

| 方法                                                         | 退货 | 描述                                                         |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| `run(<HTMLElement> *el*, <Point> *newPos*,<Number> *duration?*, <Number>*easeLinearity?*)` | ``   | 运行给定元素的动画到一个新的位置，任选地设置持续时间秒（`0.25`默认）和宽松的线性因子（的第三参数[三次Bezier曲线](http://cubic-bezier.com/#0,0,.5,1)， `0.5`默认情况下）。 |
| `stop()`                                                 | ``   | 停止动画（如果当前正在运行）。                               |

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 可拖动

用于使DOM元素可拖动（包括触摸支持）的类。在内部用于地图和标记拖动。仅适用于以定位的元素[`L.DomUtil.setPosition`](https://leafletjs.com/reference-1.2.0.html#domutil-setposition)。

### 使用范例

```
var draggable = new L.Draggable(elementToDrag);
draggable.enable();
```

### 建设者

| 建设者                                                       | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `L.Draggable(<HTMLElement> *el*, <HTMLElement> *dragHandle?*,<Boolean> *preventOutline?*, <Draggable options> *options?*)` | 开始拖动元素时创建一个[`Draggable`](https://leafletjs.com/reference-1.2.0.html#draggable)移动对象（默认情况下等于自身）。`el``dragHandle``el` |

### 选件

| 选项                 | 类型     | 默认 | 描述                                                         |
| -------------------- | -------- | ---- | ------------------------------------------------------------ |
| `clickTolerance` | `Number` | `3`  | 用户在单击期间可以移动鼠标指针的最大像素数，以将其视为有效点击（与鼠标拖动相反）。 |

### 事件

| 事件             | 数据           | 描述                                                   |
| ---------------- | -------------- | ------------------------------------------------------ |
| `down `      | `Event`        | 当拖动即将开始时触发。                                 |
| `dragstart ` | `Event`        | 拖动开始时触发                                         |
| `predrag `   | `Event`        | *在*每次相应更新元素位置*之前，*在拖动过程中连续触发。 |
| `drag `      | `Event`        | 拖动期间连续发射。                                     |
| `dragend `   | `DragEndEvent` | 拖动结束时触发。                                       |

### 方法

| 方法            | 退货 | 描述         |
| --------------- | ---- | ------------ |
| `enable()`  | ``   | 启用拖动功能 |
| `disable()` | ``   | 禁用拖动能力 |

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 类

L.Class为Leaflet的OOP设施提供动力，并用于创建此处记录的几乎所有Leaflet类。除了实现简单的经典继承模型外，它还引入了一些特殊的属性以方便代码组织-选项，包含和静态。

### 使用范例

```
var MyClass = L.Class.extend({
initialize: function (greeter) {
    this.greeter = greeter;
    // class constructor
},
greet: function (name) {
    alert(this.greeter + ', ' + name)
    }
});
// create instance of MyClass, passing "Hello" to the constructor
var a = new MyClass("Hello");
// call greet method, alerting "Hello, World"
a.greet("World");
```

#### 阶级工厂

您可能已经注意到，不使用`new`关键字创建Leaflet对象。这是通过用小写的工厂方法补充每个类来实现的：

```
new L.Map('map'); // becomes:
L.map('map');
```

工厂的实现非常容易，您可以针对自己的班级执行以下操作：

```
L.map = function (id, options) {
    return new L.Map(id, options);
};
```

#### 遗产

您可以使用L.Class.extend定义新类，但可以在任何类上使用相同的方法来继承新类：

```
var MyChildClass = MyClass.extend({
    // ... new properties and methods
});
```

这将创建一个类，该类继承父类的所有方法和属性（通过适当的原型链），并添加或覆盖传递给扩展的那些方法和属性。它还将对instanceof做出适当的反应：

```
var a = new MyChildClass();
a instanceof MyChildClass; // true
a instanceof MyClass; // true
```

您可以通过访问父类原型并使用JavaScript的调用或应用，从相应的子方法（包括其他语言的超级调用）中调用父方法（包括构造函数）：

```
var MyChildClass = MyClass.extend({
    initialize: function () {
        MyClass.prototype.initialize.call(this, "Yo");
    },
    greet: function (name) {
        MyClass.prototype.greet.call(this, 'bro ' + name + '!');
    }
});
var a = new MyChildClass();
a.greet('Jason'); // alerts "Yo, bro Jason!"
```

#### 选件

`options`是一个特殊属性，与传递给其他对象的其他对象不同，`extend`它将与父对象合并而不是完全覆盖它，这使管理对象的配置和默认值变得很方便：

```
var MyClass = L.Class.extend({
    options: {
        myOption1: 'foo',
        myOption2: 'bar'
    }
});
var MyChildClass = MyClass.extend({
    options: {
        myOption1: 'baz',
        myOption3: 5
    }
});
var a = new MyChildClass();
a.options.myOption1; // 'baz'
a.options.myOption2; // 'bar'
a.options.myOption3; // 5
```

还有[`L.Util.setOptions`](https://leafletjs.com/reference-1.2.0.html#util-setoptions)一种方法，用于方便地将传递给构造函数的选项与默认定义的类进行合并：

```
var MyClass = L.Class.extend({
    options: {
        foo: 'bar',
        bla: 5
    },
    initialize: function (options) {
        L.Util.setOptions(this, options);
        ...
    }
});
var a = new MyClass({bla: 10});
a.options; // {foo: 'bar', bla: 10}
```

#### 包括

`includes` 是一个特殊的类属性，它将所有指定的对象合并到该类中（此类对象称为mixins）。

```
 var MyMixin = {
    foo: function () { ... },
    bar: 5
};
var MyClass = L.Class.extend({
    includes: MyMixin
});
var a = new MyClass();
a.foo();
```

您还可以使用以下`include`方法在运行时进行此类包含：

```
MyClass.include(MyMixin);
```

`statics` 只是一个方便属性，将指定的对象属性作为类的静态属性注入，可用于定义常量：

```
var MyClass = L.Class.extend({
    statics: {
        FOO: 'bar',
        BLA: 5
    }
});
MyClass.FOO; // 'bar'
```

#### 构造钩

如果您是插件开发人员，则通常需要向现有类中添加其他初始化代码（例如，编辑的钩子[`L.Polyline`](https://leafletjs.com/reference-1.2.0.html#polyline)）。Leaflet提供了一种使用以下`addInitHook`方法轻松完成此操作的方法：

```
MyClass.addInitHook(function () {
    // ... do something in constructor additionally
    // e.g. add event listeners, set custom properties etc.
});
```

当您只需要进行其他方法调用时，也可以使用以下快捷方式：

```
MyClass.addInitHook('methodName', arg1, arg2, …);
```

### 功能

| 功能                                  | 退货       | 描述                                                         |
| ------------------------------------- | ---------- | ------------------------------------------------------------ |
| `extend(<Object> *props*)`        | `Function` | 给定要包含的属性，[扩展当前类](https://leafletjs.com/reference-1.2.0.html#class-inheritance)。返回作为类构造函数的Javascript函数（将通过调用`new`）。 |
| `include(<Object>*properties*)`   | `this`     | 在当前类中[包括一个mixin](https://leafletjs.com/reference-1.2.0.html#class-includes)。 |
| `mergeOptions(<Object>*options*)` | `this`     | [合并`options`](https://leafletjs.com/reference-1.2.0.html#class-options)到该类的默认值中。 |
| `addInitHook(<Function> *fn*)`    | `this`     | 向类添加[构造函数挂钩](https://leafletjs.com/reference-1.2.0.html#class-constructor-hooks)。 |

## 事件

在事件驱动的类（如[`Map`](https://leafletjs.com/reference-1.2.0.html#map)和[`Marker`](https://leafletjs.com/reference-1.2.0.html#marker)）之间共享的一组方法。通常，事件使您可以在对象发生某些事情时执行某些功能（例如，用户单击地图，从而引发地图`'click'`事件）。

### 使用范例

```
map.on('click', function(e) {
    alert(e.latlng);
} );
```

Leaflet通过引用处理事件侦听器，因此，如果要添加侦听器然后将其删除，请将其定义为一个函数：

```
function onClick(e) { ... }
map.on('click', onClick);
map.off('click', onClick);
```

### 方法

| 方法                                                         | 退货      | 描述                                                         |
| ------------------------------------------------------------ | --------- | ------------------------------------------------------------ |
| `on(<String> *type*,<Function> *fn*, <Object>*context?*)` | `this`    | 将侦听器函数（`fn`）添加到对象的特定事件类型。您可以选择指定侦听器的上下文（此关键字将指向的对象）。您还可以传递几种以空格分隔的类型（例如`'click dblclick'`）。 |
| `on(<Object> *eventMap*)`                                | `this`    | 添加一组类型/侦听器对，例如 `{click: onClick, mousemove: onMouseMove}` |
| `off(<String> *type*,<Function> *fn?*, <Object>*context?*)` | `this`    | 删除以前添加的侦听器功能。如果未指定功能，它将从对象中删除该特定事件的所有侦听器。请注意，如果您向传递了自定义上下文`on`，则必须将相同的上下文传递给`off`，以删除侦听器。 |
| `off(<Object> *eventMap*)`                               | `this`    | 删除一组类型/侦听器对。                                      |
| `off()`                                                  | `this`    | 删除对象上所有事件的所有侦听器。                             |
| `fire(<String> *type*,<Object> *data?*, <Boolean>*propagate?*)` | `this`    | 触发指定类型的事件。您可以选择提供一个数据对象-侦听器函数的第一个参数将包含其属性。可以选择将事件传播给事件父级。 |
| `listens(<String> *type*)`                               | `Boolean` | 返回`true`特定事件类型是否附加了任何侦听器。                 |
| `once(*…*)`                                              | `this`    | 行为为[`on(…)`](https://leafletjs.com/reference-1.2.0.html#evented-on)，除了侦听器只会被触发一次然后被删除。 |
| `addEventParent(<Evented>*obj*)`                         | `this`    | 添加事件父级- [`Evented`](https://leafletjs.com/reference-1.2.0.html#evented)将接收传播的事件 |
| `removeEventParent(<Evented>*obj*)`                      | `this`    | 删除事件父级，因此它将停止接收传播的事件                     |
| `addEventListener(*…*)`                                  | `this`    | 别名 [`on(…)`](https://leafletjs.com/reference-1.2.0.html#evented-on) |
| `removeEventListener(*…*)`                               | `this`    | 别名 [`off(…)`](https://leafletjs.com/reference-1.2.0.html#evented-off) |
| `clearAllEventListeners(*…*)`                            | `this`    | 别名 [`off()`](https://leafletjs.com/reference-1.2.0.html#evented-off) |
| `addOneTimeEventListener(*…*)`                           | `this`    | 别名 [`once(…)`](https://leafletjs.com/reference-1.2.0.html#evented-once) |
| `fireEvent(*…*)`                                         | `this`    | 别名 [`fire(…)`](https://leafletjs.com/reference-1.2.0.html#evented-fire) |
| `hasEventListeners(*…*)`                                 | `Boolean` | 别名 [`listens(…)`](https://leafletjs.com/reference-1.2.0.html#evented-listens) |

## 层

所有Leaflet层都使用的Layer基类中的一组方法。从继承所有方法，选项和事件[`L.Evented`](https://leafletjs.com/reference-1.2.0.html#evented)。

### 使用范例

```
var layer = L.Marker(latlng).addTo(map);
layer.addTo(map);
layer.remove();
```

### 选件

| 选项              | 类型     | 默认            | 描述                                                         |
| ----------------- | -------- | --------------- | ------------------------------------------------------------ |
| `pane`        | `String` | `'overlayPane'` | 默认情况下，该图层将添加到地图的[覆盖窗格中](https://leafletjs.com/reference-1.2.0.html#map-overlaypane)。覆盖此选项将导致默认情况下将图层放置在另一个窗格上。 |
| `attribution` | `String` | `null`          | 归因控件中显示的字符串描述了图层数据，例如“©Mapbox”。        |

### 事件

| 事件          | 数据    | 描述                   |
| ------------- | ------- | ---------------------- |
| `add `    | `Event` | 将图层添加到地图后触发 |
| `remove ` | `Event` | 从地图上移除图层后触发 |

#### 弹出事件

| 事件              | 数据         | 描述                             |
| ----------------- | ------------ | -------------------------------- |
| `popupopen `  | `PopupEvent` | 当绑定到该层的弹出窗口打开时触发 |
| `popupclose ` | `PopupEvent` | 当绑定到该层的弹出窗口关闭时触发 |

#### 工具提示事件

| 事件                | 数据           | 描述                               |
| ------------------- | -------------- | ---------------------------------- |
| `tooltipopen `  | `TooltipEvent` | 当绑定到此层的工具提示打开时触发。 |
| `tooltipclose ` | `TooltipEvent` | 当绑定到此层的工具提示关闭时触发。 |

### 方法

扩展类[`L.Layer`](https://leafletjs.com/reference-1.2.0.html#layer)将继承以下方法：

| 方法                               | 退货          | 描述                                                         |
| ---------------------------------- | ------------- | ------------------------------------------------------------ |
| `addTo(<Map|LayerGroup>*map*)` | `this`        | 将图层添加到给定的地图或图层组中。                           |
| `remove()`                     | `this`        | 从当前处于活动状态的地图中删除该图层。                       |
| `removeFrom(<Map> *map*)`      | `this`        | 从给定地图中删除图层                                         |
| `getPane(<String> *name?*)`    | `HTMLElement` | 返回`HTMLElement`表示地图上命名窗格的。如果`name`省略，则返回该层的窗格。 |
| `getAttribution()`             | `String`      | 由使用`attribution control`，返回[归因选项](https://leafletjs.com/reference-1.2.0.html#gridlayer-attribution)。 |

#### 扩展方式

每层都应从[`L.Layer`](https://leafletjs.com/reference-1.2.0.html#layer)以下方法扩展和（重新）实现。

| 方法                        | 退货     | 描述                                                         |
| --------------------------- | -------- | ------------------------------------------------------------ |
| `onAdd(<Map> *map*)`    | `this`   | 应该包含为该图层创建DOM元素的代码，将其添加到`map panes`它们应属于的位置，并将侦听器置于相关的地图事件上。呼吁[`map.addLayer(layer)`](https://leafletjs.com/reference-1.2.0.html#map-addlayer)。 |
| `onRemove(<Map>*map*)`  | `this`   | 应该包含所有清理代码，这些代码将从DOM中删除该图层的元素，并删除先前在中添加的侦听器[`onAdd`](https://leafletjs.com/reference-1.2.0.html#layer-onadd)。呼吁[`map.removeLayer(layer)`](https://leafletjs.com/reference-1.2.0.html#map-removelayer)。 |
| `getEvents()`           | `Object` | 此可选方法应返回一个对象，例如`{ viewreset: this._reset }`for [`addEventListener`](https://leafletjs.com/reference-1.2.0.html#evented-addeventlistener)。该对象中的事件处理程序将自动与图层一起从地图中添加和删除。 |
| `getAttribution()`      | `String` | 此可选方法应返回一个包含HTML的字符串，该字符串将在`Attribution control`可见该层时显示。 |
| `beforeAdd(<Map>*map*)` | `this`   | 可选方法。[`map.addLayer(layer)`](https://leafletjs.com/reference-1.2.0.html#map-addlayer)在将图层添加到地图之前，事件初始化之前调用，而无需等待地图处于可用状态。仅用于早期初始化。 |

#### 弹出方法

所有层共享一组方便将弹出窗口绑定到其的方法。

```
var layer = L.Polygon(latlngs).bindPopup('Hi There!').addTo(map);
layer.openPopup();
layer.closePopup();
```

单击该图层时，弹出窗口也将自动打开；从地图上删除该图层或打开另一个弹出窗口时，弹出窗口也将自动关闭。

| 方法                                                         | 退货      | 描述                                                         |
| ------------------------------------------------------------ | --------- | ------------------------------------------------------------ |
| `bindPopup(<String|HTMLElement|Function|Popup>*content*, <Popup options> *options?*)` | `this`    | 将弹出窗口与传递的内容绑定到该层，`content`并设置必要的事件侦听器。如果`Function`传递了a ，它将接收该层作为第一个参数，并应返回`String`或`HTMLElement`。 |
| `unbindPopup()`                                          | `this`    | 删除先前与绑定的弹出窗口`bindPopup`。                        |
| `openPopup(<LatLng> *latlng?*)`                          | `this`    | [`latlng`](https://leafletjs.com/reference-1.2.0.html#latlng)如果未`latlng`传递，则在指定的弹出窗口或默认弹出窗口锚点处打开绑定弹出窗口。 |
| `closePopup()`                                           | `this`    | 如果该图层处于打开状态，则关闭绑定到该图层的弹出窗口。       |
| `togglePopup()`                                          | `this`    | 根据其当前状态打开或关闭绑定到该层的弹出窗口。               |
| `isPopupOpen()`                                          | `boolean` | 返回`true`绑定到该层的弹出窗口当前是否打开。                 |
| `setPopupContent(<String|HTMLElement|Popup>*content*)`   | `this`    | 设置绑定到此层的弹出窗口的内容。                             |
| `getPopup()`                                             | `Popup`   | 返回绑定到此层的弹出窗口。                                   |

#### 工具提示方法

所有层共享一组便于将工具提示绑定到它的方法。

```
var layer = L.Polygon(latlngs).bindTooltip('Hi There!').addTo(map);
layer.openTooltip();
layer.closeTooltip();
```

| 方法                                                         | 退货      | 描述                                                         |
| ------------------------------------------------------------ | --------- | ------------------------------------------------------------ |
| `bindTooltip(<String|HTMLElement|Function|Tooltip>*content*, <Tooltip options> *options?*)` | `this`    | 将工具提示与传递的内容绑定到该层，`content`并设置必要的事件侦听器。如果`Function`传递了a ，它将接收该层作为第一个参数，并应返回`String`或`HTMLElement`。 |
| `unbindTooltip()`                                        | `this`    | 删除先前与绑定的工具提示`bindTooltip`。                      |
| `openTooltip(<LatLng> *latlng?*)`                        | `this`    | [`latlng`](https://leafletjs.com/reference-1.2.0.html#latlng)如果未`latlng`通过，则在指定的或默认的工具提示锚点处打开绑定的工具提示。 |
| `closeTooltip()`                                         | `this`    | 如果该工具栏处于打开状态，则将其关闭。                       |
| `toggleTooltip()`                                        | `this`    | 根据其当前状态打开或关闭绑定到该层的工具提示。               |
| `isTooltipOpen()`                                        | `boolean` | 返回`true`当前绑定到该层的工具提示是否打开。                 |
| `setTooltipContent(<String|HTMLElement|Tooltip>*content*)` | `this`    | 设置绑定到该层的工具提示的内容。                             |
| `getTooltip()`                                           | `Tooltip` | 返回绑定到该层的工具提示。                                   |

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 互动层

[`Layer`](https://leafletjs.com/reference-1.2.0.html#layer)可以使某些组件具有交互性-当用户与此类层交互时，鼠标事件类似`click`且`mouseover`可以处理。使用[事件处理方法](https://leafletjs.com/reference-1.2.0.html#evented-method)来处理这些事件。

### 选件

| 选项                      | 类型      | 默认   | 描述                                                         |
| ------------------------- | --------- | ------ | ------------------------------------------------------------ |
| `interactive`         | `Boolean` | `true` | 如果为`false`，则该图层将不会发出鼠标事件，并且将充当基础地图的一部分。 |
| `bubblingMouseEvents` | `Boolean` | `true` | 设为时`true`，此层上的鼠标事件将触发地图上的同一事件（除非[`L.DomEvent.stopPropagation`](https://leafletjs.com/reference-1.2.0.html#domevent-stoppropagation)使用）。 |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的选项

### 事件

#### 鼠标事件

| 事件              | 数据         | 描述                                                         |
| ----------------- | ------------ | ------------------------------------------------------------ |
| `click `      | `MouseEvent` | 当用户单击（或点击）图层时触发。                             |
| `dblclick `   | `MouseEvent` | 当用户双击（或双击）图层时触发。                             |
| `mousedown `  | `MouseEvent` | 当用户在图层上按下鼠标按钮时触发。                           |
| `mouseover `  | `MouseEvent` | 当鼠标进入图层时触发。                                       |
| `mouseout `   | `MouseEvent` | 当鼠标离开图层时触发。                                       |
| `contextmenu` | `MouseEvent` | 当用户右键单击图层时触发，阻止默认浏览器上下文菜单显示，如果此事件上有侦听器。当用户按住一次触摸（也称为长按）时，也会在移动设备上触发。 |

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出事件

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示事件

### 方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的弹出方法

▶从[图层](https://leafletjs.com/reference-1.2.0.html#layer)继承的工具提示方法

▶从[事件](https://leafletjs.com/reference-1.2.0.html#evented)继承的方法

## 控制

L.Control是用于实现地图控件的基类。处理定位。所有其他控件都从此类扩展。

### 选件

| 选项           | 类型     | 默认         | 描述                                                         |
| -------------- | -------- | ------------ | ------------------------------------------------------------ |
| `position` | `String` | `'topright'` | 控件的位置（地图角之一）。可能的值是`'topleft'`， `'topright'`，`'bottomleft'`或者`'bottomright'` |

### 方法

扩展L.Control的类将继承以下方法：

| 方法                                   | 退货          | 描述                                   |
| -------------------------------------- | ------------- | -------------------------------------- |
| `getPosition()`                    | `string`      | 返回控件的位置。                       |
| `setPosition(<string> *position*)` | `this`        | 设置控件的位置。                       |
| `getContainer()`                   | `HTMLElement` | 返回包含控件的HTMLElement。            |
| `addTo(<Map> *map*)`               | `this`        | Adds the control to the given map.     |
| `remove()`                         | `this`        | 从当前处于活动状态的地图中删除该控件。 |

#### 扩展方式

每个控件应从[`L.Control`](https://leafletjs.com/reference-1.2.0.html#control)以下方法扩展和（重新）实现。

| 方法                       | 退货          | 描述                                                         |
| -------------------------- | ------------- | ------------------------------------------------------------ |
| `onAdd(<Map>*map*)`    | `HTMLElement` | 应该返回控件的容器DOM元素，并在相关的地图事件上添加侦听器。呼吁[`control.addTo(map)`](https://leafletjs.com/reference-1.2.0.html#control-addTo)。 |
| `onRemove(<Map>*map*)` | ``            | 可选方法。应该包含所有清除代码，这些代码将删除先前添加到中的侦听器[`onAdd`](https://leafletjs.com/reference-1.2.0.html#control-onadd)。呼吁[`control.remove()`](https://leafletjs.com/reference-1.2.0.html#control-remove)。 |

## 处理程序

地图交互处理程序的抽象类

### 方法

| 方法            | 退货      | 描述                       |
| --------------- | --------- | -------------------------- |
| `enable()`  | `this`    | 启用处理程序               |
| `disable()` | `this`    | 禁用处理程序               |
| `enabled()` | `Boolean` | 返回`true`是否启用处理程序 |

#### 扩展方式

继承自的类[`Handler`](https://leafletjs.com/reference-1.2.0.html#handler)必须实现以下两个方法：

| 方法                | 退货 | 描述                                             |
| ------------------- | ---- | ------------------------------------------------ |
| `addHooks()`    | ``   | 启用处理程序时调用，应添加事件挂钩。             |
| `removeHooks()` | ``   | 在禁用处理程序时调用，应删除之前添加的事件挂钩。 |

## 投影

一种具有将世界的地理坐标投影到平坦表面（和背面）上的方法的对象。请参阅[地图投影](http://en.wikipedia.org/wiki/Map_projection)。

### 方法

| 方法                             | 退货     | 描述                                                         |
| -------------------------------- | -------- | ------------------------------------------------------------ |
| `project(<LatLng> *latlng*)` | `Point`  | 将地理坐标投影到2D点。仅接受实际[`L.LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng)实例，不接受数组。 |
| `unproject(<Point> *point*)` | `LatLng` | 的逆`project`。将2D点投影到地理位置中。仅接受实际[`L.Point`](https://leafletjs.com/reference-1.2.0.html#point)实例，不接受数组。 |

### 性质

| 属性          | 类型     | 描述                            |
| ------------- | -------- | ------------------------------- |
| `bounds ` | `Bounds` | 投影有效的范围（以CRS单位指定） |

### 确定的预测

Leaflet附带了一组已经定义好的Projection：

| 投影                                 | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| `L.Projection.LonLat`            | Equirectangular, or Plate Carree projection — the most simple projection, mostly used by GIS enthusiasts. Directly maps `x` as longitude, and `y` as latitude. Also suitable for flat worlds, e.g. game maps. Used by the `EPSG:4326` and `Simple` CRS. |
| `L.Projection.Mercator`          | Elliptical Mercator projection — more complex than Spherical Mercator. Takes into account that Earth is a geoid, not a perfect sphere. Used by the EPSG:3395 CRS. |
| `L.Projection.SphericalMercator` | Spherical Mercator projection — the most common projection for online maps, used by almost all free and commercial tile providers. Assumes that Earth is a sphere. Used by the `EPSG:3857` CRS. |

## CRS

### Methods

| Method                                                 | Returns        | Description                                                  |
| ------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| `latLngToPoint(<LatLng> *latlng*,<Number> *zoom*)` | `Point`        | Projects geographical coordinates into pixel coordinates for a given zoom. |
| `pointToLatLng(<Point> *point*,<Number> *zoom*)`   | `LatLng`       | The inverse of `latLngToPoint`. Projects pixel coordinates on a given zoom into geographical coordinates. |
| `project(<LatLng> *latlng*)`                       | `Point`        | Projects geographical coordinates into coordinates in units accepted for this CRS (e.g. meters for EPSG:3857, for passing it to WMS services). |
| `unproject(<Point> *point*)`                       | `LatLng`       | Given a projected coordinate returns the corresponding LatLng. The inverse of `project`. |
| `scale(<Number> *zoom*)`                           | `Number`       | Returns the scale used when transforming projected coordinates into pixel coordinates for a particular zoom. For example, it returns `256 * 2^zoom` for Mercator-based CRS. |
| `zoom(<Number> *scale*)`                           | `Number`       | Inverse of `scale()`, returns the zoom level corresponding to a scale factor of `scale`. |
| `getProjectedBounds(<Number>*zoom*)`               | `Bounds`       | Returns the projection's bounds scaled and transformed for the provided `zoom`. |
| `distance(<LatLng> *latlng1*,<LatLng> *latlng2*)`  | `Number`       | Returns the distance between two geographical coordinates.   |
| `wrapLatLng(<LatLng> *latlng*)`                    | `LatLng`       | Returns a [`LatLng`](https://leafletjs.com/reference-1.2.0.html#latlng) where lat and lng has been wrapped according to the CRS's `wrapLat` and `wrapLng` properties, if they are outside the CRS's bounds. |
| `wrapLatLngBounds(<LatLngBounds>*bounds*)`         | `LatLngBounds` | Returns a [`LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds) with the same size as the given one, ensuring that its center is within the CRS's bounds. Only accepts actual [`L.LatLngBounds`](https://leafletjs.com/reference-1.2.0.html#latlngbounds) instances, not arrays. |

### Properties

| Property       | Type       | Description                                                  |
| -------------- | ---------- | ------------------------------------------------------------ |
| `code `    | `String`   | Standard code name of the CRS passed into WMS services (e.g. `'EPSG:3857'`) |
| `wrapLng ` | `Number[]` | An array of two numbers defining whether the longitude (horizontal) coordinate axis wraps around a given range and how. Defaults to `[-180, 180]` in most geographical CRSs. If `undefined`, the longitude axis does not wrap around. |
| `wrapLat ` | `Number[]` | Like `wrapLng`, but for the latitude (vertical) axis.        |
| `infinite` | `Boolean`  | If true, the coordinate space will be unbounded (infinite in both axes) |

### Defined CRSs

| CRS                  | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `L.CRS.EPSG3395` | Rarely used by some commercial tile providers. Uses Elliptical Mercator projection. |
| `L.CRS.EPSG3857` | The most common CRS for online maps, used by almost all free and commercial tile providers. Uses Spherical Mercator projection. Set in by default in Map's [`crs`](https://leafletjs.com/reference-1.2.0.html#crs) option. |
| `L.CRS.EPSG4326` | A common CRS among GIS enthusiasts. Uses simple Equirectangular projection. Leaflet 1.0.x complies with the [TMS coordinate scheme for EPSG:4326](https://wiki.osgeo.org/wiki/Tile_Map_Service_Specification#global-geodetic), which is a breaking change from 0.7.x behaviour. If you are using a [`TileLayer`](https://leafletjs.com/reference-1.2.0.html#tilelayer) with this CRS, ensure that there are two 256x256 pixel tiles covering the whole earth at zoom level zero, and that the tile coordinate origin is (-180,+90), or (-180,-90) for `TileLayer`s with [the `tms` option](https://leafletjs.com/reference-1.2.0.html#tilelayer-tms) set. |
| `L.CRS.Earth`    | Serves as the base for CRS that are global such that they cover the earth. Can only be used as the base for other CRS and cannot be used directly, since it does not have a `code`, [`projection`](https://leafletjs.com/reference-1.2.0.html#projection) or [`transformation`](https://leafletjs.com/reference-1.2.0.html#transformation). `distance()` returns meters. |
| `L.CRS.Simple`   | A simple CRS that maps longitude and latitude into `x` and `y` directly. May be used for maps of flat surfaces (e.g. game maps). Note that the `y` axis should still be inverted (going from bottom to top). `distance()` returns simple euclidean distance. |
| `L.CRS.Base`     | Object that defines coordinate reference systems for projecting geographical points into pixel (screen) coordinates and back (and to coordinates in other units for [WMS](https://en.wikipedia.org/wiki/Web_Map_Service) services). See [spatial reference system](http://en.wikipedia.org/wiki/Coordinate_reference_system). Leaflet defines the most usual CRSs by default. If you want to use a CRS not defined by default, take a look at the [Proj4Leaflet](https://github.com/kartena/Proj4Leaflet) plugin. |

## Renderer

Base class for vector renderer implementations ([`SVG`](https://leafletjs.com/reference-1.2.0.html#svg), [`Canvas`](https://leafletjs.com/reference-1.2.0.html#canvas)). Handles the DOM container of the renderer, its bounds, and its zoom animation. A [`Renderer`](https://leafletjs.com/reference-1.2.0.html#renderer) works as an implicit layer group for all [`Path`](https://leafletjs.com/reference-1.2.0.html#path)s - the renderer itself can be added or removed to the map. All paths use a renderer, which can be implicit (the map will decide the type of renderer and use it automatically) or explicit (using the [`renderer`](https://leafletjs.com/reference-1.2.0.html#renderer) option of the path). Do not use this class directly, use `SVG` and `Canvas` instead.

### Options

| Option        | Type     | Default | Description                                                  |
| ------------- | -------- | ------- | ------------------------------------------------------------ |
| `padding` | `Number` | `0.1`   | How much to extend the clip area around the map view (relative to its size) e.g. 0.1 would be 10% of map view in each direction |

▶ Options inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### Events

| Event        | Data    | Description                                                  |
| ------------ | ------- | ------------------------------------------------------------ |
| `update` | `Event` | Fired when the renderer updates its bounds, center and zoom, for example when its map has moved |

▶ Events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Popup events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Tooltip events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### Methods

▶ Methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Popup methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Tooltip methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Methods inherited from [Evented](https://leafletjs.com/reference-1.2.0.html#evented)

## Event objects

Whenever a class inheriting from [`Evented`](https://leafletjs.com/reference-1.2.0.html#evented) fires an event, a listener function will be called with an event argument, which is a plain object containing information about the event. For example:

```
map.on('click', function(ev) {
    alert(ev.latlng); // ev is an event object (MouseEvent in this case)
});
```

The information available depends on the event type:

### Event

The base event object. All other event objects contain these properties too.

| Property      | Type     | Description                      |
| ------------- | -------- | -------------------------------- |
| `type `   | `String` | The event type (e.g. `'click'`). |
| `target ` | `Object` | The object that fired the event. |

### KeyboardEvent

| Property             | Type       | Description                                                  |
| -------------------- | ---------- | ------------------------------------------------------------ |
| `originalEvent ` | `DOMEvent` | The original [DOM ](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)[`KeyboardEvent`](https://leafletjs.com/reference-1.2.0.html#keyboardevent) that triggered this Leaflet event. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### MouseEvent

| Property             | Type       | Description                                                  |
| -------------------- | ---------- | ------------------------------------------------------------ |
| `latlng `        | `LatLng`   | The geographical point where the mouse event occured.        |
| `layerPoint `    | `Point`    | Pixel coordinates of the point where the mouse event occured relative to the map layer. |
| `containerPoint` | `Point`    | Pixel coordinates of the point where the mouse event occured relative to the map сontainer. |
| `originalEvent ` | `DOMEvent` | The original [DOM ](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent)[`MouseEvent`](https://leafletjs.com/reference-1.2.0.html#mouseevent) or [DOM `TouchEvent`](https://developer.mozilla.org/en-US/docs/Web/API/TouchEvent) that triggered this Leaflet event. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### LocationEvent

| Property               | Type           | Description                                                  |
| ---------------------- | -------------- | ------------------------------------------------------------ |
| `latlng `          | `LatLng`       | Detected geographical location of the user.                  |
| `bounds `          | `LatLngBounds` | Geographical bounds of the area user is located in (with respect to the accuracy of location). |
| `accuracy `        | `Number`       | Accuracy of location in meters.                              |
| `altitude `        | `Number`       | Height of the position above the WGS84 ellipsoid in meters.  |
| `altitudeAccuracy` | `Number`       | Accuracy of altitude in meters.                              |
| `heading `         | `Number`       | The direction of travel in degrees counting clockwise from true North. |
| `speed `           | `Number`       | Current velocity in meters per second.                       |
| `timestamp `       | `Number`       | The time when the position was acquired.                     |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### ErrorEvent

| Property       | Type     | Description                 |
| -------------- | -------- | --------------------------- |
| `message ` | `String` | Error message.              |
| `code `    | `Number` | Error code (if applicable). |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### LayerEvent

| Property     | Type    | Description                          |
| ------------ | ------- | ------------------------------------ |
| `layer ` | `Layer` | The layer that was added or removed. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### LayersControlEvent

| Property     | Type     | Description                                      |
| ------------ | -------- | ------------------------------------------------ |
| `layer ` | `Layer`  | The layer that was added or removed.             |
| `name `  | `String` | The name of the layer that was added or removed. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### TileEvent

| Property      | Type          | Description                                                  |
| ------------- | ------------- | ------------------------------------------------------------ |
| `tile `   | `HTMLElement` | The tile element (image).                                    |
| `coords ` | `Point`       | Point object with the tile's `x`, `y`, and `z` (zoom level) coordinates. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### TileErrorEvent

| Property      | Type          | Description                                                  |
| ------------- | ------------- | ------------------------------------------------------------ |
| `tile `   | `HTMLElement` | The tile element (image).                                    |
| `coords ` | `Point`       | Point object with the tile's `x`, `y`, and `z` (zoom level) coordinates. |
| `error `  | `*`           | Error passed to the tile's `done()` callback.                |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### ResizeEvent

| Property       | Type    | Description                          |
| -------------- | ------- | ------------------------------------ |
| `oldSize ` | `Point` | The old size before resize event.    |
| `newSize ` | `Point` | The new size after the resize event. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### GeoJSONEvent

| Property            | Type     | Description                                                  |
| ------------------- | -------- | ------------------------------------------------------------ |
| `layer `        | `Layer`  | The layer for the GeoJSON feature that is being added to the map. |
| `properties `   | `Object` | GeoJSON properties of the feature.                           |
| `geometryType ` | `String` | GeoJSON geometry type of the feature.                        |
| `id `           | `String` | GeoJSON ID of the feature (if present).                      |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### PopupEvent

| Property     | Type    | Description                          |
| ------------ | ------- | ------------------------------------ |
| `popup ` | `Popup` | The popup that was opened or closed. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### TooltipEvent

| Property       | Type      | Description                            |
| -------------- | --------- | -------------------------------------- |
| `tooltip ` | `Tooltip` | The tooltip that was opened or closed. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### DragEndEvent

| Property        | Type     | Description                                                |
| --------------- | -------- | ---------------------------------------------------------- |
| `distance ` | `Number` | The distance in pixels the draggable element was moved by. |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

### ZoomAnimEvent

| Property        | Type      | Description                                                  |
| --------------- | --------- | ------------------------------------------------------------ |
| `center `   | `LatLng`  | The current center of the map                                |
| `zoom `     | `Number`  | The current zoom level of the map                            |
| `noUpdate ` | `Boolean` | Whether layers should update their contents due to this event |

▶ Properties inherited from [Event](https://leafletjs.com/reference-1.2.0.html#event)

## DivOverlay

Base model for L.Popup and L.Tooltip. Inherit from it for custom popup like plugins.

### Options

| Option          | Type     | Default       | Description                                                  |
| --------------- | -------- | ------------- | ------------------------------------------------------------ |
| `offset`    | `Point`  | `Point(0, 7)` | The offset of the popup position. Useful to control the anchor of the popup when opening it on some overlays. |
| `className` | `String` | `''`          | A custom CSS class name to assign to the popup.              |
| `pane`      | `String` | `'popupPane'` | `Map pane` where the popup will be added.                    |

▶ Options inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### Events

▶ Events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Popup events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Tooltip events inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

### Methods

▶ Methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Popup methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Tooltip methods inherited from [Layer](https://leafletjs.com/reference-1.2.0.html#layer)

▶ Methods inherited from [Evented](https://leafletjs.com/reference-1.2.0.html#evented)

## Global Switches

Global switches are created for rare cases and generally make Leaflet to not detect a particular browser feature even if it's there. You need to set the switch as a global variable to true before including Leaflet on the page, like this:

```
<script>L_NO_TOUCH = true;</script>
<script src="leaflet.js"></script>
```

| Switch         | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `L_NO_TOUCH`   | Forces Leaflet to not use touch events even if it detects them. |
| `L_DISABLE_3D` | 强制Leaflet不要使用硬件加速的CSS 3D变换进行定位（这在某些罕见的环境中可能会引起故障），即使它们受支持也是如此。 |

## noConflict

此方法将`L`全局变量恢复为包含Leaflet之前的原始值，并返回真实的Leaflet名称空间，因此您可以将其放置在其他位置，如下所示：

```
<script src='libs/l.js'>
<!-- L points to some other library -->
<script src='leaflet.js'>
<!-- you include Leaflet, it replaces the L variable to Leaflet namespace -->
<script>
var Leaflet = L.noConflict();
// now L points to that other library again, and you can use Leaflet.Map etc.
</script>
```

## 版

代表正在使用的Leaflet版本的常数。

```
L.version; // contains "1.0.0" (or whatever version is currently in use)
```

[分级为4 +](http://agafonkin.com/en) ©2010–2019 [Vladimir Agafonkin](http://agafonkin.com/en)。地图© [OpenStreetMap](https://www.openstreetmap.org/copyright)提供者。