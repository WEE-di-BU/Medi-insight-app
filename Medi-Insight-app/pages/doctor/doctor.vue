<template>
	<view class="wrapper">
		<view class="header">
			<view class="text">
				<text style="font-size: 40rpx;">心内科</text>
			</view>
		</view>
		<view class="doc-list">
			<view class="doc" v-for="(doctor, index) in doctors" :key="index" @click="goToRegistration(doctor)">
				<view class="left">
					<image class="doc-img" :src="doctor.image"></image>
				</view>
				<view class="right">
					<view class="first">
						<text class="name">{{ doctor.username }}</text>
						<text class="level" style="font-size: 35rpx;margin-left: 30rpx;">{{ doctor.isExpert ? '主任医生' : '主治医生' }}</text>
					</view>
					<view class="second">
						<view class="box" style="background-color: lightgreen;">
							<text class="type">医保</text>
						</view>
						<view class="box" style="background-color: skyblue;">
							<text class="type">视频门诊</text>
						</view>
						<view class="box" style="background-color: orange;">
							<text class="type">实时热门</text>
						</view>
					</view>
					<view class="introduction" style="margin-top: 20rpx;">
						<text>性别: {{ doctor.gender }}</text>
					</view>
					<view class="introduction" style="margin-top: 20rpx;">
						<text>年龄: {{ doctor.age }}</text>
					</view>
					<view class="introduction" style="margin-top: 20rpx;">
						<text>地址: {{ doctor.address }}</text>
					</view>
					<view class="introduction" style="margin-top: 10rpx;">
						<text>电话: {{ doctor.phone }}</text>
					</view>
					<view class="introduction" style="margin-top: 20rpx;">
						<text >邮箱: {{ doctor.email }}</text>
					</view>
					<view class="introduction" style="margin-top: 20rpx;">
						<text>科室: {{ doctor.department }}</text>
					</view>
					<view class="cost" style="margin-top: 10rpx;">
						<text>挂号费：</text>
						<text style="color: red;font-size: 40rpx;">￥{{ doctor.fee }}</text>
					</view>
				</view>
			</view>
		</view>
	</view>
	<aiLogalVue />
</template>

<script>
	import aiLogalVue from '../ai/ai-logal.vue';
	export default {
		components:{
			aiLogalVue,
		},
		data() {
			return {
				doctors: [] // 初始为空，数据从后端获取
			};
		},		
		methods: {
			goToRegistration(doctor) {
				uni.navigateTo({
					url: `/pages/registration/registration?doctor=${JSON.stringify(doctor)}`
				});
			},
			async fetchDoctors() {
				try {
					const response = await uni.request({
						url: 'http://localhost:8080/auth/api/get-doctors', // 后端接口地址
						method: 'GET'
					});
					if (response.statusCode === 200) {
						this.doctors = response.data.map(doctor => ({
							...doctor,
							image: '/static/doctor-img/doc.png',
							fee: '10.0'
						})); // 将后端数据与静态默认值整合
					} else {
						console.error('Failed to fetch doctors:', response);
					}
				} catch (error) {
					console.error('Error fetching doctors:', error);
				}
			},
		},
		created() {
			this.fetchDoctors(); // 在组件创建时调用接口获取数据
		},
	}
</script>

<style>
	
.header{
	display: flex;
	background-color: #f0effd;
	align-items: center;
	justify-content: center;
}
.text{
	margin: 20rpx;
}
.doc-list {
    display: flex;
    flex-direction: column;
    gap: 20rpx;
    padding: 20rpx; 
    background-color: #ffffff; 
    border-radius: 10rpx; 
}

.doc {
    display: flex;
    padding: 20rpx;
    background-color: #ffffff; 
    border-radius: 10rpx; 
    box-shadow: 0 5rpx 10rpx rgba(0, 0, 0, 0.1); 
    transition: transform 0.2s ease-in-out;
}

.doc:hover {
    transform: scale(1.02); /* 鼠标悬浮时轻微放大 */
}

.doc .left {
    flex: 0 0 120rpx;
    margin-right: 20rpx;
}

.doc-img {
    width: 100%;
    height: 100%;
    border-radius: 50%; /* 图片显示为圆形 */
    object-fit: cover;
}

.doc .right {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.right .first {
    display: flex;
    align-items: center;
}

.name {
    font-size: 36rpx;
    font-weight: bold;
    color: #333333;
}

.level {
    font-size: 30rpx;
    color: #777777;
}

.second {
    display: flex;
    flex-wrap: wrap;
    gap: 10rpx;
    margin-top: 10rpx;
}

.box {
    padding: 5rpx 3rpx;
    border-radius: 5rpx;
    font-size: 26rpx;
    color: #ffffff;
	display: flex;
}

.type {
    font-weight: bold;
}

.introduction {
    margin-top: 10rpx;
    font-size: 28rpx;
    color: #555555;
}

.cost {
    font-size: 30rpx;
    font-weight: bold;
    color: #000000;
}

.cost text {
    color: #e74c3c; /* 挂号费的数字为红色 */
}
.left{
	flex: 2;
}
.doc-img{
	width: 110rpx;
	height: 110rpx;
}
.name{
	font-size: 40rpx; 
	border: 10rpx;
}
.second{
	display: flex;
	margin-top: 20rpx;
}
.box{
	border-radius: 10rpx;
	height: 50rpx;
	margin-right: 20rpx;
}
.type{
	color: white;
	margin: 10rpx;
	align-items: center;
	justify-content: center;
}
.right{
	flex: 7;
	margin-right: 25rpx;
}
</style>