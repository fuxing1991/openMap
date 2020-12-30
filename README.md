### 使用说明
本插件适合用于H5，APP，微信小程序开发时唤起手机安装的地图APP进行导航；因为微信公众号页面不能调起外部地图APP，只能使用公众号自己的SDK，可自行到微信公众平台:<a href="https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html#37" target="_blank">使用微信内置地图查看位置接口</a>查看(使用过程中遇到问题可以加交流群：865939377咨询)

<p style="color:red">注：微信小程序使用需在当前页面添加map组件;微信小程序只能传入终点位置信息进行导航，不能自定义起点。</p>

### 使用方式
#### 路线规划
```
<map id="map"></map> //微信小程序页面添加

import Map from '@/js_sdk/fx-openMap/openMap.js'
//既有起点也有终点
var options = {
	origin:{  //导航起点坐标和名称,如果不传则起点为当前位置
		latitude:39.92848272,
		longitude:116.39560823,
		name:"起点名称"
	},
	destination:{  //导航终点点坐标和名称
		latitude:39.98848272,
		longitude:116.47560823,
		name:"终点名称"
	},
	mode:"drive", //导航方式 公交：bus驾车：drive（默认）,步行：walk,骑行：bike
	mapId:"map" //map 组件的 id (微信小程序端需传入)
}
//只有有终点(起点默认为当前位置) 微信小程序使用方式
var options = {
	destination:{  //导航终点点坐标和名称
		latitude:39.98848272,
		longitude:116.47560823,
		name:"终点名称"
	},
	mode:"drive", //导航方式 公交：bus驾车：drive（默认）,步行：walk,骑行：bike
	mapId:"map" //map 组件的 id (微信小程序端需传入)
}
Map.routePlan(options,type)

```

##### 属性说明:


| 参数				| 类型		| 说明															| 是否必选	|
| ---------------	| ----		| ------------------------										| ----		|
| options			| Object	|																| 是		|
| ┣ origin			| Object	| 导航起点坐标和名称,如果不传则起点为当前位置						| 否		|
| ┣ ┣ latitude		| Float		| 纬度（默认GCJ-02坐标系）										| 是		|
| ┣ ┣ longitude		| Float		| 经度（默认GCJ-02坐标系）										| 是		|
| ┣ ┣ name			| String	| 地图标注名称													| 否		|
| ┣ destination		| Object	| 导航终点点坐标和名称											| 是		|
| ┣ ┣ latitude		| Float		| 纬度（默认GCJ-02坐标系）										| 是		|
| ┣ ┣ longitude		| Float		| 经度（默认GCJ-02坐标系）										| 是		|
| ┣ ┣ name			| String	| 地图标注名称													| 否		|
| ┣ mode			|  String	| 导航方式 公交：bus驾车：drive（默认）,步行：walk,骑行：bike		| 否		|
| ┣ mapId			|  String	| map 组件的 id (微信小程序端需传入)									| 否		|
| type				| String	| 地图坐标系类型【“gcj02(默认)”、“wgs84”、“bd09”】					| 否		|

#### 驾车导航（直接调起地图APP导航；注：腾讯地图和微信小程序只能实现路径规划不能实现直接导航）
```
<map id="map"></map> //小程序页面添加

import Map from '@/js_sdk/fx-openMap/openMap.js'
var options = {
	destination:{  //导航终点点坐标和名称
		latitude:39.98848272,
		longitude:116.47560823,
		name:"终点名称"
	},
	mapId:"map" //map 组件的 id (微信小程序端需传入)
}
Map.navigation(options,type)

```

##### 属性说明:


| 参数				| 类型		| 说明															| 是否必选	|
| ---------------	| ----		| ------------------------										| ----		|
| options			| Object	|																| 是		|
| ┣ destination		| Object	| 导航终点点坐标和名称											| 是		|
| ┣ ┣ latitude		| Float		| 纬度（默认GCJ-02坐标系）										| 是		|
| ┣ ┣ longitude		| Float		| 经度（默认GCJ-02坐标系）										| 是		|
| ┣ ┣ name			| String	| 地图标注名称													| 否		|
| ┣ mapId			|  String	| map 组件的 id (微信小程序端需传入)									| 否		|
| type				| String	| 地图坐标系类型【“gcj02(默认)”、“wgs84”、“bd09”】					| 否		|

