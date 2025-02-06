<template>
	<view class="personal-page">
		<view class="header">
			<view class="avatar-container" @click="showLoginModal">
				<image v-if="!isLoggedIn" class="avatar" src="/static/user-img/hui.png"></image>
				<image v-else class="avatar" :src="user.avatar"></image>
				<text v-if="!isLoggedIn" class="login-status" style="color: white;">未登录</text>
				<text v-else class="login-status" style="color: white;">{{ user.name }}</text>
			</view>
		</view>
		<view class="content">
			<view class="options">
				<view class="menu-item" @click="goToPersonalMsg">
					<text>个人信息</text>
					<text class="arrow">></text>
				</view>
				<view class="menu-item">
					<text>账号安全</text>
					<text class="arrow">></text>
				</view>
				<view class="menu-item">
					<text>关于我们</text>
					<text class="arrow">></text>
				</view>
				<view class="menu-item">
					<text>使用帮助</text>
					<text class="arrow">></text>
				</view>
				<view class="menu-item" @click="logout">
					<text>退出登录</text>
					<text class="arrow">></text>
				</view>
			</view>
			<view style="margin-top: 550rpx;display: flex; align-items: center;justify-content: center;">
				<text style="color: darkgray">欢迎登录Medi-Insight系统！</text>
			</view>
		</view>

		<!-- 用户信息展示 -->
		<!-- 登录弹窗 -->
		<view class="modal" v-if="showModal">
			<view class="modal-content">
				<text class="modal-title">登录</text>
				<input class="modal-input" placeholder="请输入账号" v-model="loginId" />
				<input class="modal-input" type="password" placeholder="请输入密码" />
				<button class="modal-button" @click="handleLogin">登录</button>
				<button class="modal-button" @click="handleCancel">取消</button>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			loginId: '', // 登录输入的身份证号
			showModal: false,
			isLoggedIn: false,
			user: {
				name: '呃呃呃',
				sex: '',
				age: '',
				birth: '',
				merrage: '未婚',
				realName: '',
				identifyType: '居民身份证',
				identifyNumber: '990881200001012212',
				country: '中国',
				nation: '汉',
				phone: '90876543210',
				disease: '冠心病',
				avatar: '/static/user-img/touxiang.png',
				email: ''
			}
		};
	},
	methods: {
		showLoginModal() {
			this.showModal = true;
		},
		handleLogin() {
			// 设置身份证号为全局变量
			uni.setStorageSync('userId', this.loginId);
			this.fetchUserInfo(this.loginId); // 调用后端接口获取用户数据
		},
		handleCancel() {
			this.showModal = false;
		},
		async fetchUserInfo(id) {
			try {
				const response = await uni.request({
					url: `http://localhost:8080/auth/api/patients/${id}`,
					method: 'GET'
				});

				if (response.statusCode === 200) {
					const data = response.data;

					// 更新用户信息
					this.user.realName = data.username;
					this.user.age = data.age;
					this.user.sex = data.gender;
					this.user.email = data.email;
					this.user.birth = data.birthday;

					this.isLoggedIn = true; // 更新登录状态
					this.showModal = false; // 隐藏登录弹窗
				} else {
					console.error('Failed to fetch user data:', response);
					uni.showToast({
						title: '获取用户信息失败',
						icon: 'none'
					});
				}
			} catch (error) {
				console.error('Error fetching user data:', error);
				uni.showToast({
					title: '网络请求失败',
					icon: 'none'
				});
			}
		},
		logout() {
			this.isLoggedIn = false;
			this.loginId = '';
			uni.removeStorageSync('userId');
			this.user = {
				name: '呃呃呃',
				sex: '',
				age: '',
				birth: '',
				merrage: '未婚',
				realName: '',
				identifyType: '居民身份证',
				identifyNumber: '990881200001012212',
				country: '中国',
				nation: '汉',
				phone: '90876543210',
				disease: '冠心病',
				avatar: '/static/user-img/touxiang.png',
				email: ''
			};
		},
		goToPersonalMsg() {
			if (this.isLoggedIn) {
				uni.navigateTo({
					url: `/pages/personalMsg/personalMsg?userInfo=${JSON.stringify(this.user)}`
				});
			} else {
				uni.showToast({
					title: '请先登录',
					icon: 'none'
				});
			}
		}
	},
	onShow() {
		const userId = uni.getStorageSync('userId');
		if (userId) {
			this.fetchUserInfo(userId); // 自动登录已保存的用户
		}
	}
};
</script>

<style>
.personal-page {
	display: flex;
	flex-direction: column;
	height: 100vh;
}

.header {
	padding: 20rpx;
	height: 250rpx;
	background-image: linear-gradient(to right, #C088C4, #87B1E0);
	display: flex;
	align-items: center;
	justify-content: center;
}

.avatar-container {
	display: flex;
	flex-direction: column;
	align-items: center;
	margin-right: 20rpx;
}

.avatar {
	width: 100rpx;
	height: 100rpx;
	border-radius: 50%;
}

.login-status {
	margin-top: 10rpx;
	font-size: 28rpx;
	color: #666;
}

.content {
	flex: 1;
	padding: 20rpx;
	background-color: aliceblue;
}

.options {
	background-color: white;
	border-top-left-radius: 15rpx;
	border-top-right-radius: 15px;
	border-bottom-left-radius: 15rpx;
	border-bottom-right-radius: 15px;
}

.menu-item {
	display: flex;
	height: 70rpx;
	justify-content: space-between;
	align-items: center;
	padding: 20rpx;
	border-bottom: 1px solid #ccc;
}

.arrow {
	font-size: 30rpx;
	color: #999;
}

.modal {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	background-color: rgba(0, 0, 0, 0.5);
	display: flex;
	justify-content: center;
	align-items: center;
}

.modal-content {
	background-color: white;
	padding: 20rpx;
	border-radius: 10rpx;
	width: 80%;
	display: flex;
	flex-direction: column;
	align-items: center;
}

.modal-title {
	font-size: 36rpx;
	color: #333;
	margin-bottom: 20rpx;
}

.modal-input {
	width: 100%;
	font-size: 30rpx;
	padding: 10rpx;
	border: 1px solid #ccc;
	border-radius: 10rpx;
	outline: none;
	margin-bottom: 20rpx;
}

.modal-button {
	width: 100%;
	background-color: #C088C4;
	color: white;
	border: none;
	border-radius: 10rpx;
	padding: 10rpx 20rpx;
	font-size: 30rpx;
	margin-bottom: 10rpx;
}
</style>