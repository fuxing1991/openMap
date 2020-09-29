### 使用说明
本插件适合用于H5，APP,小程序开发时唤起手机安装的地图APP进行导航；因为微信公众号有自己的SDK，可自行到微信公众平台查看
(使用过程中遇到问题可以加我扣扣：493000217咨询)

### 使用方式

```
import Map from '@/js_sdk/fx-openMap/openMap.js'
var options = {
	origin:{  //导航起点坐标和名称,如果不传则起点为当前位置
		latitude:39.92848272,
		longitude:116.39560823,
		name:"起点"
	},
	destination:{  //导航终点点坐标和名称
		latitude:39.98848272,
		longitude:116.47560823,
		name:"终点"
	},
	mode:"drive"  //导航方式 公交：bus驾车：drive（默认）,步行：walk,骑行：bike
}
Map.openMap(options,type)

```

### 属性说明:


| 参数            | 类型 | 说明                    | 是否必选 |
| --------------- | ---- | ------------------------ | ---- |
| options        | Object  |                   | 是 |
| ┣ origin        | Object  | 导航起点坐标和名称,如果不传则起点为当前位置                   | 否 |
| ┣ ┣ latitude        | Float  | 纬度（默认GCJ-02坐标系）                   | 是 |
| ┣ ┣ longitude  | Float   | 经度（默认GCJ-02坐标系）                   | 是 |
| ┣ ┣ name      | String  | 地图标注名称                 | 否 |
| ┣ destination        | Object  | 导航终点点坐标和名称                 | 是 |
| ┣ ┣ latitude        | Float  | 纬度（默认GCJ-02坐标系）                   | 是 |
| ┣ ┣ longitude  | Float   | 经度（默认GCJ-02坐标系）                   | 是 |
| ┣ ┣ name      | String  | 地图标注名称                 | 否 |
| ┣ mode        |  String | //导航方式 公交：bus驾车：drive（默认）,步行：walk,骑行：bike                   | 否 |
| type      | String  | 地图坐标系类型【“gcj02(默认)”、“wgs84”、“bd09”】              | 否 |

