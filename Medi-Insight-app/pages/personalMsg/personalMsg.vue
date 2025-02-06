<template>
	<view class="wrapper">
		<view class="content">
			<view class="options">
				<view class="menu-item">
					<text>头像</text>
					<view class="right-content">
						<image class="avatar" :src="userInfo.avatar"></image>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNickname">
					<text>昵称</text>
					<view class="right-content">
						<text class="info">{{ nickname }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNicksex">
					<text>性别</text>
					<view class="right-content">
						<text class="info">{{ nicksex }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNickbirth">
					<text>出生日期</text>
					<view class="right-content">
						<text class="info">{{ nickbirth }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNicksex">
					<text>疾病史</text>
					<view class="right-content">
						<text class="info">{{ userInfo.disease }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNickmerrage">
					<text>婚姻状况</text>
					<view class="right-content">
						<text class="info">{{ nickmerrage }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item">
					<text>真实姓名</text>
					<view class="right-content">
						<text class="info">{{ userInfo.realName }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item">
					<text>证件类型</text>
					<view class="right-content">
						<text class="info">{{ userInfo.identifyType }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item">
					<text>身份证号</text>
					<view class="right-content">
						<text class="info">{{ userInfo.identifyNumber }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNickcountry">
					<text>国籍</text>
					<view class="right-content">
						<text class="info">{{ nickcountry }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNicknation">
					<text>民族</text>
					<view class="right-content">
						<text class="info">{{ nicknation }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNickphone">
					<text>电话</text>
					<view class="right-content">
						<text class="info">{{ nickphone }}</text>
						<text class="arrow">></text>
					</view>
				</view>
				<view class="menu-item" @click="goToEditNickemail">
					<text>邮箱</text>
					<view class="right-content">
						<text class="info">{{ nickemail }}</text>
						<text class="arrow">></text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {},
				nickname: '',
				nicksex: '',
				nickbirth: '',
				nickmerrage: '',
				nickcountry: '',
				nicknation: '',
				nickphone: '',
				nickemail:''
			};
		},
		onLoad(options) {
			this.userInfo = JSON.parse(options.userInfo);
			this.nickname = uni.getStorageSync('nickname') || this.userInfo.name;
			this.nicksex = uni.getStorageSync('nicksex') || this.userInfo.sex;
			this.nickbirth = uni.getStorageSync('nickbirth') || this.userInfo.birth;
			this.nickmerrage = uni.getStorageSync('nickmerrage') || this.userInfo.merrage;
			this.nickcountry = uni.getStorageSync('nickcountry') || this.userInfo.country;
			this.nicknation = uni.getStorageSync('nicknation') || this.userInfo.nation;
			this.nickphone = uni.getStorageSync('nickphone') || this.userInfo.phone;
			this.nickemail = uni.getStorageSync('nickemail') || this.userInfo.email;
		},
		methods: {
			goToEditNickname() {
				uni.navigateTo({
					url: '/pages/personalMsg/v-name'
				});
			},
			updateNickname(newNickname) {
				this.nickname = newNickname;
				uni.setStorageSync('nickname', newNickname);
			},
			
			goToEditNicksex() {
				uni.navigateTo({
					url: '/pages/personalMsg/sex'
				});
			},
			updateNicksex(newNicksex) {
				this.nicksex = newNicksex;
				uni.setStorageSync('nicksex', newNicksex);
			},
			
			goToEditNickbirth() {
				uni.navigateTo({
					url: '/pages/personalMsg/birth'
				});
			},
			updateNickbirth(newNickbirth) {
				this.nickbirth = newNickbirth;
				uni.setStorageSync('nickbirth', newNickbirth);
			},
			
			goToEditNickmerrage() {
				uni.navigateTo({
					url: '/pages/personalMsg/merrage'
				});
			},
			updateNickmerrage(newNickmerrage) {
				this.nickmerrage = newNickmerrage;
				uni.setStorageSync('nickmerrage', newNickmerrage);
			},
			
			goToEditNickcountry() {
				uni.navigateTo({
					url: '/pages/personalMsg/country'
				});
			},
			updateNickcountry(newNickcountry) {
				this.nickcountry = newNickcountry;
				uni.setStorageSync('nickcountry', newNickcountry);
			},
			
			goToEditNicknation() {
				uni.navigateTo({
					url: '/pages/personalMsg/nation'
				});
			},
			updateNicknation(newNicknation) {
				this.nicknation = newNicknation;
				uni.setStorageSync('nicknation', newNicknation);
			},
			
			goToEditNickphone() {
				uni.navigateTo({
					url: '/pages/personalMsg/phone'
				});
			},
			updateNickphone(newNickphone) {
				this.nickphone = newNickphone;
				uni.setStorageSync('nickphone', newNickphone);
			},
			goToEditNickemail() {
				uni.navigateTo({
					url: '/pages/personalMsg/email'
				});
			},
			updateNickemail(newNickemail) {
				this.nickemaile = newNickemail;
				uni.setStorageSync('nickemail', newNickemail);
			},
		},
		onShow() {
			const newNickname = uni.getStorageSync('nickname');
			const newNicksex = uni.getStorageSync('nicksex');
			const newNickbirth = uni.getStorageSync('nickbirth');
			const newNickmerrage = uni.getStorageSync('nickmerrage');
			const newNickcountry = uni.getStorageSync('nickcountry');
			const newNicknation = uni.getStorageSync('nicknation');
			const newNickphone = uni.getStorageSync('nickphone');
			const newNickemail = uni.getStorageSync('nickemail');
			if (newNickname && newNickname !== this.nickname) {
				this.updateNickname(newNickname);
			}
			if (newNicksex && newNicksex !== this.nicksex) {
				this.updateNicksex(newNicksex);
			}
			if (newNickbirth && newNickbirth !== this.nickbirth) {
				this.updateNickbirth(newNickbirth);
			}
			if (newNickmerrage && newNickmerrage !== this.nickmerrage) {
				this.updateNickmerrage(newNickmerrage);
			}
			if (newNickcountry && newNickcountry !== this.nickcountry) {
				this.updateNickcountry(newNickcountry);
			}
			if (newNicknation && newNicknation !== this.nicknation) {
				this.updateNicknation(newNicknation);
			}
			if (newNickphone && newNickphone !== this.nickphone) {
				this.updateNickphone(newNickphone);
			}
			if (newNickemail && newNickemail !== this.nickemail) {
				this.updateNickemail(newNickemail);
			}
		}
	}
</script>

<style>
.wrapper{
	background-color: aliceblue;
	height: 1550rpx;
}
.content {
	flex: 1;
	padding: 20rpx;
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

.right-content {
	display: flex;
	align-items: center;
}

.avatar {
	width: 50rpx;
	height: 50rpx;
	border-radius: 50%;
	margin-right: 10rpx;
}

.info {
	color: darkgray;
	margin-right: 10rpx;
}

.arrow {
	font-size: 30rpx;
	color: #999;
}
</style>