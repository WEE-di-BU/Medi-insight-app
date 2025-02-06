<template>
	<view class="wrapper">
		<view class="content">
			<image class="logo" src="/static/index_img/logo-white.png"></image>
			<view class="text-area">
				<text class="title">{{ title }}</text>
			</view>
		</view>
		<view class="label">
			<view class="l-text">
				<text class="l-Ename">Medi-Insight</text>
			</view>
			<view class="l-text">
				<text class="l-Cname">AI赋能，医疗预测系统</text>
			</view>
		</view>
		<view class="back-img">
			<image class="glob" src="/static/index_img/Medi-Insight.png"></image>
		</view>
		<button class="doctor-button" @click="goToAppointment">进入挂号系统</button>
		<button class="home-button" @click="goToPerson">进入个人信息</button>
		<view v-for="(star, index) in stars" :key="index" :style="star.style" class="star"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Medi-Insight',
				starCount: 15, // 星星点点的数量
				fallSpeedMin: 1, // 最小坠落速度
				fallSpeedMax: 5, // 最大坠落速度
				stars: []
			};
		},
		mounted() {
			this.createStars();
		},
		methods: {
			goToAppointment() {
				uni.navigateTo({
					url: '/pages/doctor/doctor' // 注意路径
				});
			},
			goToPerson() {
				uni.navigateTo({
					url: '/pages/person/person' // 注意路径
				});
			},
			createStars() {
				for (let i = 0; i < this.starCount; i++) {
					const fallSpeed = Math.random() * (this.fallSpeedMax - this.fallSpeedMin) + this.fallSpeedMin;
					this.stars.push({
						style: {
							left: `${Math.random() * 100}%`,
							top: `${Math.random() * -100}px`,
							animation: `fall ${fallSpeed}s linear infinite`
						}
					});
				}
			}
		}
	}
</script>

<style>
	.wrapper{
		background-color: #f0effd;
		height: 100%;
	}
	.content {
		display: flex;
		align-items: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
		margin-left: -40rpx;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}

	.back-img {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.glob {
		height: 700rpx;
		width: 1230rpx;
		margin-left: -490rpx;
		margin-bottom: 10rpx;
	}

	.l-text {
		display: flex;
		margin-top: 20rpx;
		align-items: center;
		justify-content: center;
	}

	.l-Ename {
		background-image: linear-gradient(to right, #7D26CD, #B452CD, #63B8FF, #0000FF);
		-webkit-background-clip: text;
		background-clip: text;
		color: transparent;
		font-size: 2.8em;
	}

	.l-Cname {
		background-image: linear-gradient(to right, #7D26CD, #B452CD, #63B8FF, #0000FF);
		-webkit-background-clip: text;
		background-clip: text;
		color: transparent;
		font-size: 1.3em;
	}

	/* 按钮样式设置 */
	.doctor-button {
		width: 50%;
		background-image: linear-gradient(to right, #C088C4, #87B1E0);
		color: white;
		margin-top: 95rpx;
		font-size: 1.2em;
		border: none;
		border-radius: 10px;
		padding: 10px 20px;
		box-shadow: 0 0 5px rgba(125, 38, 205, 0.6);
	}

	.doctor-button:hover {
		background-image: linear-gradient(to right, #C088C4, #87B1E0);
		box-shadow: 0 0 8px rgba(125, 38, 205, 0.8);
	}
	.home-button {
		width: 50%;
		background-image: linear-gradient(to right, #C088C4, #87B1E0);
		color: white;
		margin-top: 50rpx;
		font-size: 1.2em;
		border: none;
		border-radius: 10px;
		padding: 10px 20px;
		box-shadow: 0 0 5px rgba(125, 38, 205, 0.6);
	}
	
	.home-button:hover {
		background-image: linear-gradient(to right, #C088C4, #87B1E0);
		box-shadow: 0 0 8px rgba(125, 38, 205, 0.8);
	}

	@keyframes fall {
		from {
			top: -10px;
		}
		to {
			top: 110vh;
		}
	}

	.star {
		width: 4px;
		height: 4px;
		background-color:white;
		border-radius: 50%;
		position: absolute;
		opacity: 0.6;
	}

	.star::before {
		content: '';
		position: absolute;
		top: 0;
		left: 50%;
		width: 2px; /* 长尾的宽度 */
		height: 100px; /* 长尾的长度 */
		background: linear-gradient(to top, rgba(170, 170, 255, 0.8), transparent); /* 长尾的渐变效果 */
		transform: translateX(-50%); /* 使长尾居中 */
	}
</style>