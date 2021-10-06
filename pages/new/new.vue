<template>
	<div class="wrapper">
		<div class="container">
			<h1>新建卡片</h1>
			<form class="form" @submit="submit">
				<picker mode="selector" :range="typeList" :value="typeIndex" @change="typeChange" name="type">
					<view class="input">{{typeList[typeIndex]}}</view>
				</picker>
				<input type="text" placeholder="标题" name="title">
				<picker mode="date" @change="dateChange" name="startDate" :value="startDate" 
				:start="typeIndex === 0 ? currentDate : '1999-11-13'"  :end="typeIndex === 1 ? currentDate : '99999-11-13'">
					<view class="input">{{startDate}}</view>
				</picker>
				<view class="input">
					是否置顶
					<switch name="top" color="rgb(0,150,136)" />
				</view>
				<button form-type="submit" id="login-button">添加<text class="iconfont icon-jia"/></button>
			</form>
		</div>
		<ul class="bg-bubbles">
			<li></li>
			<li></li>
			<li></li>
			<li></li>
			<li></li>
			<li></li>
			<li></li>
			<li></li>
			<li></li>
			<li></li>
		</ul>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				typeList: ['倒数','正数'],
				typeIndex: 0,
				startDate: '目标日期',
				timmer: null,
				date: {}
			}
		},
		computed:{
			currentDate(){
				if(JSON.stringify(this.date) !== '{}'){
					let Y = this.date.getFullYear();
					let M = this.date.getMonth()+1;
					let D = this.date.getDate();
					return Y + (M < 10 ? "-0" : "-") + M + (D < 10 ? "-0" : "-") + D;
				}
				else{
					return '';
				}
			}
		},
		methods: {
			typeChange(e){
				this.typeIndex = e.detail.value;
			},
			submit(formData){
				const data = formData.detail.value;
				const {title,startDate,top,type} = data;
				console.log(data)
				if(title === '' || startDate === '目标日期'){
					uni.showToast({
						title: '表单填写不完整， 提交失败',
						icon: 'none',
						duration: 3000
					})
				}else{
					let list = uni.getStorageSync('list');
					const newItem = {title,startDate,type,id: title + startDate};
					if(top){
						let top = uni.getStorageSync('top');
						if(Object.keys(top).length !=0){
							list = [top,...list];
							uni.setStorage({
									key: 'list',
									data: list
								})
						}
						uni.setStorage({
							key: 'top',
							data: newItem,
							success() {
								uni.showToast({
									title: '添加成功',
									complete() {
										setTimeout(()=>{
											uni.navigateTo({
											    url: '../index/index'
											});
										},2000)
										
									}
								})
							}
						})
					}else{
						uni.setStorage({
							key: 'list',
							data: [...list,newItem],
							success() {
								uni.showToast({
									title: '添加成功',
									complete() {
										setTimeout(()=>{
											uni.redirectTo({
												url: '../index/index'											    
											});
										},1500)
									}
								})
							}
						})
					}
				}	
			},
			dateChange(data){
				this.startDate = data.detail.value;
			}
		},
		created() {
			this.timmer = setInterval(()=>{
				this.date = new Date();
			},1000)
		}
	}
</script>

<style scoped>
	.input>switch{
		margin-left: 100rpx;
		width: 60rpx;
		
	}
	.wrapper {
		color: white;
		font-weight: 300;
		background: linear-gradient(to bottom right, #50a3a2 0%, #53e3a6 100%);
		position: absolute;
		left: 0;
		width: 100%;
		height: 100vh;
		overflow: hidden;
	}

	.wrapper.form-success .container h1 {
		transform: translateY(170rpx);
	}

	.container {
		max-width: 1200rpx;
		margin: 0 auto;
		padding: 200rpx 0;
		height: 100vh;
		text-align: center;
	}

	.container h1 {
		font-size: 80rpx;
		transition-duration: 1s;
		transition-timing-function: ease-in-put;
		font-weight: 200;
	}

	form {
		padding: 40rpx 0;
		position: relative;
		z-index: 2;
	}

	form input,.input {
		appearance: none;
		outline: 0;
		border: 2rpx solid rgba(255, 255, 255, 0.4);
		background-color: rgba(255, 255, 255, 0.2);
		width: 500rpx;
		border-radius: 6rpx;
		padding: 20rpx 30rpx;
		margin: 0 auto 20rpx auto;
		display: block;
		text-align: center;
		font-size: 36rpx;
		color: white;
		transition-duration: 0.25s;
		font-weight: 300;
	}


	form input:focus {
		background-color: white;
		width: 600rpx;
		color: #53e3a6;
	}

	form button {
		appearance: none;
		outline: 0;
		background-color: white;
		border: 0;
		padding: 20rpx 30rpx;
		color: #53e3a6;
		border-radius: 6rpx;
		width: 500rpx;
		cursor: pointer;
		font-size: 36rpx;
		transition-duration: 0.25s;
	}





	.bg-bubbles {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 1;
		pointer-events: none;
		padding: 0;
	}

	.bg-bubbles li {
		position: absolute;
		list-style: none;
		display: block;
		width: 80rpx;
		height: 80rpx;
		background-color: rgba(255, 255, 255, 0.15);
		bottom: -320rpx;
		animation: square 25s infinite;
		transition-timing-function: linear;
	}

	.bg-bubbles li:nth-child(1) {
		left: 10%;
	}

	.bg-bubbles li:nth-child(2) {
		left: 20%;
		width: 160rpx;
		height: 160rpx;
		animation-delay: 2s;
		animation-duration: 17s;
	}

	.bg-bubbles li:nth-child(3) {
		left: 25%;
		animation-delay: 4s;
	}

	.bg-bubbles li:nth-child(4) {
		left: 40%;
		width: 120rpx;
		height: 120rpx;
		animation-duration: 22s;
		background-color: rgba(255, 255, 255, 0.25);
	}

	.bg-bubbles li:nth-child(5) {
		left: 70%;
	}

	.bg-bubbles li:nth-child(6) {
		left: 80%;
		width: 240rpx;
		height: 240rpx;
		animation-delay: 3s;
		background-color: rgba(255, 255, 255, 0.2);
	}

	.bg-bubbles li:nth-child(7) {
		left: 32%;
		width: 320rpx;
		height: 320rpx;
		animation-delay: 7s;
	}

	.bg-bubbles li:nth-child(8) {
		left: 55%;
		width: 40rpx;
		height: 40rpx;
		animation-delay: 15s;
		animation-duration: 40s;
	}

	.bg-bubbles li:nth-child(9) {
		left: 25%;
		width: 20rpx;
		height: 20rpx;
		animation-delay: 2s;
		animation-duration: 40s;
		background-color: rgba(255, 255, 255, 0.3);
	}

	.bg-bubbles li:nth-child(10) {
		left: 90%;
		width: 320rpx;
		height: 320rpx;
		animation-delay: 11s;
	}

	@-webkit-keyframes square {
		0% {
			transform: translateY(0);
		}

		100% {
			transform: translateY(calc(-100vh - 320rpx)) rotate(600deg);
		}
	}

	@keyframes square {
		0% {
			transform: translateY(0);
		}

		100% {
			transform: translateY(calc(-100vh - 320rpx)) rotate(600deg);
		}
	}
</style>
