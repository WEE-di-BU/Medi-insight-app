<template>
  <view>
    <!-- 固定位置的圆形按钮，点击跳转到 ai.vue -->
    <view class="floating-button" @click="goToAI">
      <text class="button-text">AI</text>
    </view>
    
    <!-- 其他页面内容 -->
    <slot></slot> <!-- 保证其他页面内容能够渲染 -->
  </view>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {
    // 点击按钮跳转到 ai.vue
    goToAI() {
      uni.navigateTo({
        url: '/pages/ai/ai'  // 请确保 ai.vue 在 pages/ai 目录下
      });
    }
  }
}
</script>

<style scoped>
/* 圆形按钮的样式 */
.floating-button {
  width: 60px;
  height: 60px;
  background-image: linear-gradient(to right, #C088C4, #87B1E0);
  border-radius: 50%; /* 使其变成圆形 */
  position: fixed;
  bottom: 20px; /* 距离底部 20px */
  right: 20px; /* 距离右侧 20px */
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.3);
  z-index: 9999; /* 确保它在所有内容之上 */
}

/* 圆形按钮中的文字 */
.button-text {
  color: white;
  font-size: 18px;
  font-weight: bold;
}
</style>
