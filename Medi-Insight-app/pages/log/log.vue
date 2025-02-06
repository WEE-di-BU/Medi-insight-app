<!-- components/log.vue -->
<template>
	<view class="modal" v-if="showModal">
		<view class="modal-content">
			<text class="modal-title">登录</text>
			<input class="modal-input" placeholder="请输入身份证号" v-model="loginId" />
			<input class="modal-input" type="password" placeholder="请输入密码" v-model="loginPwd" />
			<button class="modal-button" @click="handleLogin">登录</button>
			<button class="modal-button" @click="handleCancel">取消</button>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			loginId: '',
			loginPwd: ''
		};
	},
	props: {
		showModal: {
			type: Boolean,
			default: false
		},
		users: {
			type: Array,
			default: () => []
		}
	},
	methods: {
		handleLogin() {
			const user = this.users.find(u => u.identifyNumber === this.loginId && u.pwd === this.loginPwd);
			if (user) {
				this.$emit('login-success', { ...user, avatar: user.img });
			} else {
				uni.showToast({
					title: '身份证号或密码错误',
					icon: 'none'
				});
			}
		},
		handleCancel() {
			this.$emit('login-cancel');
		}
	}
};
</script>

<style>
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