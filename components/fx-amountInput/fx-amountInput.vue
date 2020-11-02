<template>
	<view class="keyboard-main" :style="{height:height+'px'}">
		<text :style="{fontSize:parseInt((fontSize/3*2))+'px'}">{{currency}}</text>
		<view class="keyboard-content" :style="{height:height+'px'}">
			<view class="placeholder" :style="{fontSize:parseInt((fontSize/3*2))+'px',height:height+'px'}" v-if="!money">
				{{placeholder}}
			</view>
			<view class="input" :class="{zIndex:isShow}" :style="{fontSize:size+'px',height:height+'px',fontWeight:isBold?'bold':'initial'}" @click.stop="show">
				<span id="text">
					<block v-if="isFilter">
						{{filterMoney(money)}}
					</block>
					<block v-if="!isFilter">
						{{money}}
					</block>
					<view class="line" :style="{height:parseInt((height/4*3))+'px','background':btnColor}"></view>
				</span>
			</view>
		</view>
		<view class="mask" @click.stop="hide" v-if="isShow"></view>
		<view class='keyboard' :class="{noPadding:!isShow}">
		    <view class='key-row' v-if="isShow">
		        <view class='key-cell' @click.stop='_handleKeyPress(1)'>1</view>
		        <view class='key-cell' @click.stop='_handleKeyPress(2)'>2</view>
		        <view class='key-cell' @click.stop='_handleKeyPress(3)'>3</view>
				<view class='key-cell last-child' @click.stop="_handleKeyPress('delete')">
					<image class="icon" src="../../static/fx-amountInput/backspace.png" mode="aspectFill"></image>
				</view>
		    </view>
		    <view class='key-row' v-if="isShow">
		        <view class='key-cell' @click.stop='_handleKeyPress(4)'>4</view>
		        <view class='key-cell' @click.stop='_handleKeyPress(5)'>5</view>
		        <view class='key-cell' @click.stop='_handleKeyPress(6)'>6</view>
		        <view class='key-cell last-child'></view>
		    </view>
		    <view class='key-row' v-if="isShow">
		        <view class='key-cell' @click.stop='_handleKeyPress(7)'>7</view>
		        <view class='key-cell' @click.stop='_handleKeyPress(8)'>8</view>
		        <view class='key-cell' @click.stop='_handleKeyPress(9)'>9</view>
		        <view class='key-cell last-child'></view>
		    </view>
			<view class="key-zero-and-point" v-if="isShow">
				<view class="a zero" @click.stop='_handleKeyPress(0)'>0</view>
				<view class="a point" @click.stop="_handleKeyPress('.')">.</view>
				 <view class='a last-child'></view>
			</view>
		    <view class='key-confirm' :class="{big:isShow,frist:fristShow}" :style="{'background':btnColor}" @click.stop="_handleKeyPress('confirm')">
				{{confirmText}}
			</view>
		</view>
		<view id="input-text" :style="{fontSize:(size+5)+'px',fontWeight:isBold?'bold':'initial'}">
			{{filterMoney(money)}}
		</view>
	</view>
</template>

<script>
	export default{
		name:"keyBoard",
		props:{
			confirmText:{
				default:'充值',
				type:String
			},
			btnColor:{
				default:'#2B76EF',
				type:String
			},
			placeholder:{
				default:'请输入充值金额',
				type:String
			},
			currency :{
				default:'￥',
				type:String
			},
			maxNumber:{
				default:100000000000,
				type:Number
			},
			fontSize:{
				type:[String,Number],
				default:30
			},
			isBold:{
				type:Boolean,
				default:true
			},
			isFilter:{
				type:Boolean,
				default:false
			}
		},
		data(){
			return {
				fristShow:true,
				isShow:false,
				size:0,
				height:0,
				allWidth:0,
				money:''
			}
		},
		created() {
			this.height = this.fontSize;
			this.size = this.fontSize;
			var query = uni.createSelectorQuery().in(this)
			setTimeout(()=>{
				this.show()
				query.select('.keyboard-content').boundingClientRect(data => {
				  this.allWidth = data.width
				}).exec();
			},200)
		},
		watch:{
			money(val){
				this.$emit('change',parseFloat(val));
			}
		},
		methods : {
			show(){
				this.isShow = true;
			},
			hide(){
				this.fristShow = false
				this.isShow = false;
			},
			filterMoney(value){
				if(!value){
				    return ''
				}else {
				    value=value.replace(/\$\s?|(,*)/g, '')
				    return value.replace(/\B(?=(\d{3})+(?!\d))/g, ',')
				}
			},
			//处理按键
			_handleKeyPress(num) {
				if (num == -1) return false;
				switch (String(num)) {
					//小数点
					case '.':
						this._handleDecimalPoint();
						break;
					//删除键
					case 'delete':
						this._handleDeleteKey();
						break;
					//确认键
					case 'confirm':
						this._handleConfirmKey();
						break;
					default:
						this._handleNumberKey(num);
						break;
				}
			},
			//处理小数点函数
			_handleDecimalPoint() {
				//如果包含小数点，直接返回
				if (this.money.indexOf('.') > -1) return false;
				//如果小数点是第一位，补0
				if (!this.money.length)
					this.money = '0.';
				//如果不是，添加一个小数点
				else
					this.money = this.money + '.';
				var query = uni.createSelectorQuery().in(this)
				query.select('#text').boundingClientRect(data => {
					var _w = data.width;
					if(_w+Number(this.size)>this.allWidth){
						this.size -=5
					}
				}).exec();
			},
			//处理删除键
			_handleDeleteKey() {
				let S = this.money;
				//如果没有输入，直接返回
				if (!S.length) return false;
				//否则删除最后一个
				this.money = S.substring(0, S.length - 1);
				var query = uni.createSelectorQuery().in(this)
				query.select('#input-text').boundingClientRect(data => {
					var _w = data.width;
					if(_w+20<this.allWidth && this.size<this.height){
						this.size +=5
					}
				}).exec();
			},
			//处理数字
			_handleNumberKey(num) {
				if(Number(this.money+num)>this.maxNumber){
					return
				}
				let S = this.money;
				//如果有小数点且小数点位数不小于2
				if ( S.indexOf('.') > -1 && S.substring(S.indexOf('.') + 1).length < 2)
					this.money = S + num;
				if (S.indexOf('.') > -1 && S.substring(S.indexOf('.') + 1).length >= 2)
					return
				//没有小数点
				if (!(S.indexOf('.') > -1)) {
					//如果第一位是0，只能输入小数点
					if (num == 0 && S.length == 0)
						this.money = '0.';
					else {
						if (S.length && Number(S.charAt(0)) === 0) return;
						this.money = S + num;
					}
				}
				var query = uni.createSelectorQuery().in(this)
				query.select('#text').boundingClientRect(data => {
					var _w = data.width;
					if(_w+Number(this.size)>this.allWidth){
						this.size -=5
					}
				}).exec();
			},
			
			//提交
			_handleConfirmKey() {	
				let S = this.money;
				//未输入
				if (!S.length||S==0){
					uni.showToast({
						title: this.placeholder,
						icon:'none',
						duration: 1000
					});
					return false;
				}
				//将 8. 这种转换成 8.00
				if (S.indexOf('.') > -1 && S.indexOf('.') == (S.length - 1))
					S = Number(S.substring(0, S.length - 1)).toFixed(2);
				//保留两位
				S = Number(S).toFixed(2);
				this.$emit('confirm',parseFloat(S));    //提交参数
			}
		}
	}
</script>

<style lang="less" scoped>
	.keyboard-main{
		width: 100%;
		display: flex;
		justify-content: flex-start;
		align-items: flex-end;
		padding: 20rpx 0;
		box-sizing: initial;
		color: #1B1B1B;
		text{
			font-size: 24px;
			margin-right: 10px;
		}
	}
	.keyboard-content{
		width: 100%;
		height: 30px;
		position: relative;
		.placeholder{
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			color:#c0c4cc;
			display: flex;
			justify-content: flex-start;
			align-items: flex-end;
		}
		.input{
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			display: flex;
			justify-content: flex-start;
			align-items: flex-end;
			font-size: 30px;
			font-weight: bold;
			&.zIndex{
				z-index: 999;
			}
			#text{
				display: flex;
				justify-content: flex-start;
				align-items: flex-end;
			}
			.line{
				display: inline-block;
				width: 4rpx;
				height: 30px;
				border: 2rpx;
				background-color: #0063E5;
				margin-left: 8rpx;
				animation:cursorImg 1s infinite steps(1, start);
				@keyframes cursorImg {
					0%, 100% {
						opacity: 0;
					}
					50% {
						opacity: 1;
					}
				}
			}
		}
	}
	.mask{
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: 1;
	}
    .keyboard {
		flex: 1;
        position: fixed;
        bottom: 0;
        left: 0;
        width: 100%;
		background-color: #F5F5F5;
		padding: 20rpx;
		transition: all 0.6s;
		z-index: 9;
		box-sizing: border-box;
		&.noPadding{
			padding: 0;
			bottom: -100%;
		}
    }
    .keyboard .key-row {
        display: flex;
		justify-content: space-between;
        display: -webkit-flex;
        position: relative;
		box-sizing: border-box;
    }
 
    .keyboard .key-cell {
		font-size: 20px;
		width: 160rpx;
		height: 80rpx;
		/* margin-right: 20rpx; */
		margin-bottom: 20rpx;
		background-color: #fff;
		border-radius: 8rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		&.last-child{
			/* margin-right: 0px; */
			width: 170rpx;
		}
		.icon{
			width: 50rpx;
			height: 40rpx;
		}
    }
  
    .keyboard .key-confirm {
        position: fixed;
        text-align: center;
        /* min-height: 100rpx; */
        width: 170rpx;
		padding: 20rpx 30rpx;
		border-radius: 8rpx;
		box-sizing: border-box;
		color: #FFFFFF;
        z-index: 10;
        right: 20rpx;
		top: calc(100vh - 300rpx);
        bottom: 200rpx;
		display:flex;
		justify-content: center;
		align-items: center;
		font-size: 20px;
		letter-spacing: 4px;
		font-weight: bold;
		transition: all 0.3s;
		&.big{
			right: 20rpx;
			bottom: 20rpx;
			padding: 0 30rpx;
			bottom: 20rpx;
		}
		&.frist{
			position:absolute;
			top: auto;
			right: 20rpx;
			height: 280rpx;
			bottom: 20rpx;
		}
    }
	.key-zero-and-point{
		display: flex;
		height: 80rpx;
		justify-content: space-between;
		align-items: center;
		font-size: 20px;
		.zero{
			display: flex;
			justify-content: center;
			align-items: center;
			width: 340rpx;
			text-align: center;
			background-color: #fff;
			border-radius: 8rpx;
			height: 100%;
		}
		.point{
			display: flex;
			justify-content: center;
			align-items: center;
			background-color: #fff;
			border-radius: 8rpx;
			width: 160rpx;
			height: 100%;
		}
		.last-child{
			background-color: #fff;
			border-radius: 8rpx;
			width: 170rpx;
			height: 100%;
		}
	}
	.key-cell:active{
		color: white;  
		background: black;  //黑色
		opacity: 0.1;    //这里重要，就是通过这个透明度来设置
	}
	.a:active,.key-confirm2:active{
		color: white;
		background: black;  //黑色
		opacity: 0.1;    //这里重要，就是通过这个透明度来设置
	}
	#input-text{
		width: max-content;
		position: fixed;
		bottom: -200%;
	}
</style>
