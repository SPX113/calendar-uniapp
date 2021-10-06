<template>
	<view class="item">
		<view class="days">{{days}} <span class="tian">天{{type === 0 ? '后' : ''}}</span></view>
		<view class="left">
			<view class="title">
				{{title}}
			</view>
			<view class="date">
				{{startDate}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"Item",
		props:{
			title:{
				type: String,
				default: ''
			},
			startDate:{
				type: String,
				default: ''
			},
			type:{
				type: Number,
				default: 0
			}
		},
		computed:{
			days(){
				let time = new Date(this.startDate).getTime()/1000 - new Date().getTime()/1000;
				if(this.type !== 0){
					return -parseInt((time/86400));  
				}else{
					return parseInt((time/86400));
				}
			}
		}
	}
</script>

<style scoped>
	.item{
		background-color: rgb(255,255,255,.2);
		margin: 20rpx;
		border-radius: 20rpx;
		box-sizing: border-box;
		height: 150rpx;
		position: relative;
	}
	.item>.days{
		float: right;
		font-size: 60rpx;
		padding-top: 30rpx;
		font-weight: bold;
		margin-right: 80rpx;
	}
	.tian{
		content: "天";
		font-size: 1rem;
		position: absolute;
		margin-left: 10rpx;
	}
	.item>.left{
		margin-left: 40rpx;
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
	}
</style>
