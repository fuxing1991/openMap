<template>
	<view class="content">
		<fx-amountInput ref="kdKeyboard" @change="onChange" @confirm="onConfirm"></fx-amountInput>
	</view>
</template>

<script>
	import fxAmountInput from '@/components/fx-amountInput/fx-amountInput.vue';
	export default {
		components:{
			fxAmountInput
		},
		data() {
			return {
				
			}
		},
		methods: {
			onChange(event){
				console.log("输入数据："+event)
			},
			onConfirm(event){
				console.log("提交数据："+event)
			}
		}
	}
</script>

<style lang="less">
	page{
		background-color: #fff;
	}
	.content {
		width: 100%;
	}
</style>
