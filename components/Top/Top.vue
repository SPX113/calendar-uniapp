<template>
	<view class="top">
		<view class="days">
			{{days}} 
			<text>天{{type === 0 ? '后' : ''}}</text>
		</view>
		<view class="title"><text class="iconfont icon-zhiding"/>{{title}}</view>
		<view class="date">{{startDate}} {{weekday}}</view>
	</view>
</template>

<script>
	export default{
		name: 'Top',
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
				type:Number,
				default: 0
			}
		},
		computed:{
			days(){
				let time = new Date(this.startDate).getTime()/1000 - new Date().getTime()/1000;
				if(this.type === 0){
					return parseInt((time/86400));  
				}else{
					return -parseInt((time/86400));
				}
			},
			weekday(){
				return new Date(this.startDate).toLocaleDateString([], {weekday: 'long'})
			}
		}
	}
</script>

<style scoped>
	.top{
		background-color: rgb(255,255,255,.2);
		margin: 20rpx;
		border-radius: 20rpx;
		height: 400rpx;
		text-align: center;
		padding-top: 60rpx;
		box-sizing: border-box;
	}
	.top .title{
		text-decoration:underline;
	}
	.top .days{
		font-size: 150rpx;
		position: relative;
		font-weight: bold;
	}
	.days text{
		font-size: 1.1rem;
		position: absolute;
		top: 40rpx;
		margin-left: 20rpx;
	}
</style>
