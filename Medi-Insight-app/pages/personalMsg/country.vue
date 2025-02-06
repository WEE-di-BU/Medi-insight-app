<template>
	<view class="wrapper">
		<view class="content" style="margin: 30rpx;">
			<view class="prompt" style="margin: 10rpx;">
				<text>七天内可修改一次</text>
			</view>
			<view class="in-vname" style="margin: 10rpx;height: 80rpx;background-color: aliceblue;text-align: center;align-items: center;">
				<picker class="picker-content" style="display: flex;flex: 15;align-items: center;justify-content: center;" mode="selector" :range="sexOptions" @change="onSexChange">
					<view class="picker-text">{{ selectedCountry }}</view>
				</picker>
				<view style="margin-left: auto;">></view>
			</view>
			<view class="save-delete" style="display: flex;">
				<button class="delete" @click="cancel">取消</button>
				<button class="save" @click="save">保存</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				selectedCountry: '',
				sexOptions: ['中国','美国','英国','澳大利亚'],
			};
		},
		onLoad() {
			this.selectedCountry = uni.getStorageSync('nickcountry') || '中国';
		},
		methods: {
			cancel() {
				// 取消逻辑
				uni.navigateBack();
			},
			save() {
				// 保存逻辑
				uni.setStorageSync('nickcountry', this.selectedCountry);
				uni.navigateBack();
			},
			onSexChange(e) {
				this.selectedCountry = this.sexOptions[e.detail.value];
			}
		}
	}
</script>

<style>
.wrapper {
	display: flex;
	flex-direction: column;
	height: 100vh;
}

.content {
	flex: 1;
	display: flex;
	flex-direction: column;
	align-items: center;
}

.prompt {
	font-size: 28rpx;
	color: #666;
}

.in-vname {
	width: 100%;
	display: flex;
	align-items: center;
	justify-content: center;
}

.input-content {
	width: 100%;
	font-size: 30rpx;
	padding: 10rpx;
	border: 1px solid #ccc;
	border-radius: 10rpx;
	outline: none;
}

.save-delete {
	display: flex;
	justify-content: space-between;
	width: 100%;
	margin-top: 20rpx;
}

.delete {
	flex: 1;
	background-color: #ccc;
	color: white;
	border: none;
	border-radius: 10rpx;
	padding: 10rpx 20rpx;
	font-size: 30rpx;
	margin-right: 10rpx;
}

.save {
	flex: 1;
	background-color: #007aff;
	color: white;
	border: none;
	border-radius: 10rpx;
	padding: 10rpx 20rpx;
	font-size: 30rpx;
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
</style>