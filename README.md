### 使用说明
本插件适合用于H5，APP,小程序开发时唤起手机安装的地图APP进行导航；因为微信公众号有自己的SDK，可自行到微信公众平台查看
(使用过程中遇到问题可以加我扣扣：493000217咨询)

### 使用方式

```
import Map from '@/js_sdk/fx-openMap/openMap.js'
Map.openMap(latitude, longitude, name,type)

```

### 属性说明:


| 参数            | 说明                    | 类型 |
| --------------- | ------------------------ | ---- |
| latitude        | 纬度（默认GCJ-02坐标系）                   | Float  |
| longitude  | 经度（默认GCJ-02坐标系）                   | Float   |
| name      | 地图标注名称                 | String  |
| type      | 地图坐标系类型【“gcj02(默认)”、“wgs84”、“bd09”】              | String  |

