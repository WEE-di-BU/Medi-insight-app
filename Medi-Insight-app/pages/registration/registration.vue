<template>
	<view class="registration-page">
	
		<view class="doc">
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
					<text>科室: {{ doctor.department }}</text>
				</view>
				<view class="cost" style="margin-top: 10rpx;">
					<text>挂号费：</text>
					<text style="color: red;font-size: 40rpx;">￥{{ doctor.fee }}</text>
				</view>
			</view>
		</view>
		<!-- 下半部分固定的其他内容 -->
		<view class="fixed-content">
			<text style="font-size: 40rpx;font-weight: 900;">填写预约挂号信息</text>
		</view>
		<view class="msg-list">
			<view class="msg">
				<view class="msg-content">
					<view class="l-msg">
						<text class="left-index">就诊人</text>
					</view>
					<text style="color: gainsboro;align-content: center;">|</text>
					<view class="input-content">
						<input class="" placeholder="请输入就诊人姓名" v-model="patientName" />
					</view>
				</view>
			</view>
			<view class="msg">
				<view class="msg-content">
					<view class="l-msg">
						<text class="left-index">证件类型</text>
					</view>
					<text style="color: gainsboro;align-content: center;">|</text>
					<view class="input-content">
						<picker class="picker-content" style="display: flex;flex: 15;" mode="selector" :range="idTypes" @change="onIdTypeChange">
							<view class="picker-text">{{ selectedIdType }}</view>
						</picker>
						<view style="margin-left: auto;">></view>
					</view>
				</view>
			</view>
			<view class="msg">
				<view class="msg-content">
					<view class="l-msg">
						<text class="left-index">有效证件</text>
					</view>
					<text style="color: gainsboro;align-content: center;">|</text>
					<view class="input-content">
						<input class="input-content" placeholder="请输入有效证件号" v-model="idNumber" />
					</view>
				</view>
			</view>
			<view class="msg">
				<view class="msg-content">
					<view class="l-msg">
						<text class="left-index">手机号</text>
					</view>
					<text style="color: gainsboro;align-content: center;">|</text>
					<view class="input-content">
						<input class="input-content" placeholder="请输入正确的手机号码" v-model="phoneNumber" />
					</view>
				</view>
			</view>
			<view class="msg">
				<view class="msg-content">
					<view class="l-msg">
						<text class="left-index">就诊时间</text>
					</view>
					<text style="color: gainsboro;align-content: center;">|</text>
					<view class="input-content">
						<picker class="picker-content" style="flex: 6;" mode="date" @change="onDateChange">
							<view class="picker-text">{{ selectedDate || '请选择日期' }}</view>
						</picker>
						<picker class="picker-content" style="flex: 5;" mode="time" @change="onTimeChange">
							<view class="picker-text">{{ selectedTime || '选择时间' }}</view>
						</picker>
						<view style="margin-left: auto;">></view>
					</view>
				</view>
			</view>

			<view class="msg">
				<view class="msg-content">
					<view class="l-msg">
						<text class="left-index">结束时间</text>
					</view>
					<text style="color: gainsboro;align-content: center;">|</text>
					<view class="input-content">
						<text>就诊时长二十分钟</text>
						<!-- <text><span>&nbsp;</span>{{ selectedDate }}<span>&nbsp;&nbsp;&nbsp;</span>{{ this.endDate }}</text> -->
					</view>
				</view>
			</view>

			<view class="msg">
				<view class="msg-content description">
					<view class="l-msg" style="padding-top: 20rpx;">
						<text class="left-index">疾病描述</text>
					</view>
					<view style="height: 150rpx;">
						<input class="input-content" placeholder="请描述就诊人病症" v-model="diseaseDescription" />
					</view>
				</view>
			</view>
		</view>
		
		<view>
		  <button 
		    class="regist-button" 
		    style="width: 300rpx; border-radius: 30rpx; margin-top: 20rpx; font-weight: 900; background-color: #f0f0f0;" 
		    @click="submit"
		  >
		    确认提交
		  </button>
		</view>
	</view>
	<aiLogalVue />
</template>

<script>
	import aiLogalVue from '../ai/ai-logal.vue';
	
	export default {
		data() {
			return {
				doctor: {},
				patientName: '',
				idTypes: ['居民身份证', '护照', '港澳居民来往内地通行证','台湾来往大陆通行证'],
				selectedIdType: '居民身份证',
				idNumber: '',
				phoneNumber: '',
				selectedDate: '',
				selectedTime: '',
				diseaseDescription: '',
				bookTime: null, 
			};
		},
		components:{
			aiLogalVue,
		},
		onLoad(options) {
			this.doctor = JSON.parse(decodeURIComponent(options.doctor));
		},
		methods: {
		    onIdTypeChange(e) {
		        this.selectedIdType = this.idTypes[e.detail.value];
		    },
		    onDateChange(e) {
		        this.selectedDate = e.detail.value;
		    },
		    onTimeChange(e) {
		        this.selectedTime = e.detail.value;
		        this.calculateEndTime(); // 计算结束时间
		    },
			formatDate(date) {
			    const year = date.getFullYear();
			    const month = String(date.getMonth() + 1).padStart(2, '0');
			    const day = String(date.getDate()).padStart(2, '0');
			    const hours = String(date.getHours()).padStart(2, '0');
			    const minutes = String(date.getMinutes()).padStart(2, '0');
			    const seconds = String(date.getSeconds()).padStart(2, '0');
			    return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
			},
		    calculateEndTime(startDate) {
		        if (startDate instanceof Date) {
		            const newDate = new Date(startDate);
		            newDate.setMinutes(newDate.getMinutes() + 20);  // Add 20 minutes to the start time
		            return newDate;
		        }
		        return startDate;
		    },
		    async submit() {
		        // 获取当前时间并格式化为所需格式
		        const currentTime = new Date();
		        const bookTime = this.formatDate(currentTime);
				const userId = uni.getStorageSync('userId');
				if (!userId) {
				    uni.showToast({
				        title: '用户未登录，请先登录',
				        icon: 'none'
				    });
				    return;
				}
			    const startDate = new Date(`${this.selectedDate} ${this.selectedTime}`);
			    const endDate = this.calculateEndTime(startDate);  // 计算结束时间，返回Date对象
	
			    const formattedStartTime = this.formatDate(startDate);  
			    const formattedEndTime = this.formatDate(endDate);
		       
		        const appointmentData = {
		            pid: String(userId),  
		            did: String(this.doctor.id),  
		            status: "1",  // 预约状态
		            source: "1",  // 来源
		            bookTime: bookTime, 
		            startTime: formattedStartTime,  // 格式化后的开始时间
		            endTime: formattedEndTime,  // 格式化后的结束时间  
		        };
				console.log(JSON.stringify(appointmentData));
				
		        try {
		            // 向后端发送请求
		            const response = await uni.request({
		                url: 'http://localhost:8080/auth/api/registrations', // 替换为你的后端接口地址
		                method: 'POST',
		                data: appointmentData
		            });
		
		            if (response.statusCode === 200 && response.data === 'ok') {
		                // 如果返回结果为 "ok"，显示预约成功的提示
		                console.log('预约成功:', response.data);
		                uni.showToast({
		                    title: '预约成功',
		                    icon: 'success'
		                });
		                uni.navigateTo({
		                    url: '/pages/index/index' // 注意路径
		                });
		            } else {
		                console.error('预约失败:', response);
		                uni.showToast({
		                    title: '预约失败',
		                    icon: 'none'
		                });
		            }
		        } catch (error) {
		            console.error('Error submitting appointment:', error);
		            uni.showToast({
		                title: '预约失败',
		                icon: 'none'
		            });
		        }
		    },
			goToRegistration(doctor) {
				uni.navigateTo({
					url: `/pages/registration/registration?doctor=${encodeURIComponent(JSON.stringify(doctor))}`
				});
			},
			
		 }
	}
</script>

<style>
.registration-page {
	padding: 20rpx;
}

.header{
	display: flex;
	background-color: #f0effd;
	align-items: center;
	justify-content: center;
}
.text{
	margin: 20rpx;
}

.doc{
	display: flex;
	padding-top: 30rpx;
	padding-left: 20rpx;
	padding-bottom: 5rpx;
	margin-bottom: 2rpx;
	background-color: white;
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

.fixed-content {
	margin-top: 40rpx;
	padding: 20rpx;
	background-color: #f0f0f0;
	border-radius: 10rpx;
	text-align: center;
}
.msg-list{
	margin-top: 30rpx;
	margin-left: 60rpx;
	margin-right: 60rpx;
	align-items: center;
	justify-content: center;
}
.msg{
	background-color: #F8F8FF;
	border-radius: 10rpx;
	margin-bottom: 10rpx;
}
.msg-content{
	display: flex;
}
.l-msg{
	margin: 20rpx;
	width: 150rpx;
}
.left-index{
	font-size: 35rpx;
	font-weight: 900;
}

.input-content {
	flex: 1;
	display: flex;
	align-items: center;
	font-size: 30rpx;
	padding: 10rpx;
	border: none;
	outline: none;
}

.picker-content {
	flex: 1;
	font-size: 30rpx;
	padding: 10rpx;
	border: none;
	outline: none;
}

.picker-text {
	font-size: 30rpx;
}
.description{
	display: block;
}
</style>