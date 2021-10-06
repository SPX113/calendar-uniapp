<template>
	<view>
		<HM-dragSorts :list="list" :isLongTouch="true" :rowHeight="55"  @confirm="confirm">
			<template slot="rowContent" slot-scope="{ row }">
				<view class="row">
					<text class="iconfont icon-jian" style="color: red;padding-right: 20rpx;" @tap="deleteItem(row.id)"/>
					<text class="text">{{row.title}}</text>
				</view>
			</template>
		</HM-dragSorts>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [],
				top: {}
			}
		},
		methods: {
			confirm(e) {
				const change = this.list.splice(e.index,1);
				this.list.splice(e.moveTo,0,change[0]);
				const list = this.list.slice(1);
				const top = this.list.slice(0,1);
				uni.setStorageSync('list',list);
				uni.setStorageSync('top',top[0]);
			},
			deleteItem(id){
				const that = this;
				uni.showModal({
				    title: '提示',
				    content: '您确定要删除吗？',
				    success: function (res) {
				        if (res.confirm) {
				            const newList = that.list.filter(item => {
								return item.id !== id;
							})
							that.list = newList;
							const list = that.list.slice(1);
							const top = that.list.slice(0,1);
							uni.setStorageSync('list',list);
							uni.setStorageSync('top',top[0]);
				        }
				    }
				});
			}

		},
		created() {
			this.top = uni.getStorageSync('top');
			this.list = uni.getStorageSync('list');
			this.list.unshift(this.top);
		}
	}
</script>

<style>
	.top{
		height: 55px;
		background-color: gray;
	}
	page {
		background-color: #efeff4;
	}

	@media (prefers-color-scheme: dark) {
		page {
			background-color: #000000;
		}
	}
	.content {
		.row {
			display: flex;
			flex-direction: row;
			align-items: center;
			.text {
				font-size: 13px;
			}
		}
	}
</style>
