<template>
	<view class="content">
		<view class="text-area" @tap="openMap">
			导航
		</view>
		<!-- #ifdef MP-WEIXIN -->
		<map id="map" hidden="true"></map>
		<!-- #endif -->
	</view>
</template>

<script>
	import Map from '@/js_sdk/fx-openMap/openMap.js'
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			openMap(){
				var options = {
					origin:{  //导航起点坐标和名称,如果不传则起点为当前位置
						latitude:39.92848272,
						longitude:116.39560823,
						name:"广州塔"
					},
					destination:{  //导航终点点坐标和名称
						latitude:39.98848272,
						longitude:116.47560823,
						name:"北京天安门"
					},
					// #ifdef MP-WEIXIN
					mapId:"map",
					// #endif
					mode:"drive"  //导航方式 公交：bus驾车：drive（默认）,步行：walk,骑行：bike;
				}
				//路线规划
				//Map.routePlan(options,"gcj02")
				//驾车导航，打开地图直接开启导航，只需要传入options.destination终点信息;
				Map.navigation(options,"gcj02")
			}
		}
	}
</script>

<style lang="less">
	page{
		background-color: #F5F5F5;
	}
	.content {
		width: 100%;
		.text-area {
			height: 90rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			background-color: #fff;
		}
	}
</style>
