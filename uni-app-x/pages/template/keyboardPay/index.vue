<template>
	<view>
		<view class="u-padding-40">
			<up-button type="success" @click="showPop(true)">
				<up-icon name="red-packet"></up-icon>
				<text class="u-padding-left-10">发送1.00元红包</text>
			</up-button>
		</view>		
		<up-keyboard 
			default=""
			ref="uKeyboard" 
			mode="number" 
			:mask="true" 
			:mask-close-able="false"
			:dot-enabled="false" 
			:show="show"
			:safe-area-inset-bottom="true"
			:tooltip="false"
			@change="onChange"
			@backspace="onBackspace">
			<view>
				<view class="u-text-center u-padding-20 money">
					<text>1.00</text>
					<text class="u-font-20 u-padding-left-10">元</text>
					<view class="u-padding-10 close" data-flag="false" @tap="showPop(false)">
						<up-icon name="close" color="#333333" size="28"></up-icon>
					</view>
				</view>
				<view class="u-flex u-row-center">
					<up-message-input 
						mode="box" 
						:maxlength="6"
						:dot-fill="true"
						v-model="password"
						:disabled-keyboard="true"
						@finish="finish"
					></up-message-input>
				</view>
				<view class="u-text-center u-padding-top-10 u-padding-bottom-20 tips">支付键盘</view>
			</view>
		</up-keyboard>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show:false,
				password:''
			}
		},
		onLoad() {
			
		},
		methods: {
			onChange(val){
				if(this.password.length<6){
					this.password += val;
				}
				
				if(this.password.length>=6){
					this.pay();
				}
			},
			onBackspace(e){
				if(this.password.length>0){
					this.password = this.password.substring(0,this.password.length-1);
				}
			},
			pay(){
				uni.showLoading({
					title:'支付中'
				})
				
				setTimeout(()=>{
					uni.hideLoading();
					this.show = false;
					uni.showToast({
						icon:'success',
						title:'支付成功'
					})
				},2000);
			},
			showPop(flag = true){
				this.password = '';
				this.show = flag;
			},
			finish(){
				console.log(11111)
			}
		}
	}
</script>

<style lang="scss">
	.money{
		font-size: 80rpx;
		color: $u-warning;
		position: relative;
		
		.close{
			position: absolute;
			top: 20rpx;
			right: 20rpx;
			line-height: 28rpx;
			font-size: 28rpx;
		}
	}
	.tips{
		color:$u-tips-color;
	}
</style>