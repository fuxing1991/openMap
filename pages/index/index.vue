<template>
	<view class="content">
		<view class="text-area" @tap="openMap">
			导航
		</view>
	</view>
</template>

<script>
	import Map from '@/js_sdk/fx-openMap/openMap.js'
	export default {
		data() {
			return {
				img:""
			}
		},
		onReady() {
			
		},
		methods: {
			openMap(){
				Map.openMap(39.91526781462406, 116.40393509115786, "北京天安门","bd09")
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
	.charts{width: 750upx; height:500upx;background-color: #FFFFFF;}
</style>
