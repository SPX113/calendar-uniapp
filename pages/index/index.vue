<template>
	<view class="container">
		<view class="status_bar"></view>
		<view class="navBar">
			<navigator url="../new/new" class="iconfont icon-jia" style="float: left;" />
			<navigator url="../setting/setting" style="float: right; font-size: 55rpx;" class="iconfont icon-shezhi" />
			<text space="ensp">{{'全部 '}}<text class="iconfont icon-xialajiantouxiao" /></text>
		</view>
		<view class="content">
			<Top v-if="Object.keys(top).length != 0" :title="top.title" :startDate="top.startDate" :type="top.type"/>
			<div id="drag">
				<Item v-for="item in list" :key='item.id'
				:title="item.title"  :startDate="item.startDate" :type="item.type"/>
			</div>
		</view>
	</view>
</template>

<script>
	import Sortable from 'sortablejs';
	export default {
		name: 'index',
		data(){
			return{
				list: [],
				top: {}
			}
		},
		onShow(){
			this.top = uni.getStorageSync('top');
			this.list = uni.getStorageSync('list');
		},
		mounted() {
			let that = this;
			// #ifdef H5
			new Sortable(drag,{
				animation: 150,
				onUpdate(e){
					let change = that.list.splice(e.oldIndex,1);
					that.list.splice(e.newIndex,0,change[0]);
					uni.setStorageSync('list', that.list)
				}
			})
			// #endif
		}
	}
</script>

<style scoped>
	.container {
		width: 100%;
		min-height: 100vh;
		color: white;
		background: url(../../static/background.jpg) no-repeat;
		background-size: 100% 100%;
		background-position: top center; 
		background-attachment: fixed;
	}

	.status_bar {
		height: var(--status-bar-height);
	}

	.navBar {
		margin: 0 20rpx;
		height: 150rpx;
		line-height: 150rpx;
		text-align: center;
		font-size: 1.2rem;
		font-weight: bolder;
	}
</style>
