<template>
  <view class="chat-container">
    <!-- 问答消息显示区域 -->
    <scroll-view :scroll-y="true" class="chat-box" scroll-with-animation>
      <view v-for="(message, index) in messages" :key="index" class="message-container">
        <!-- 左侧服务端消息头像 -->
        <view v-if="!message.isUser" class="message-left">
          <image src="/static/ai-img/aiImg.png" class="avatar-img" />
          <view class="server-msg">
            <text>{{ message.text }}</text>
          </view>
        </view>

        <!-- 右侧客户端消息头像 -->
        <view v-if="message.isUser" class="message-right">
		  <image src="/static/ai-img/user.jpg" class="avatar-img" />
          <view class="user-msg">
            <text>{{ message.text }}</text>
          </view>
          <!-- <image src="/static/ai-img/user.jpg" class="avatar-img" /> -->
        </view>
      </view>
    </scroll-view>

    <!-- 输入框部分，固定在底部 -->
    <view class="input-container">
      <input v-model="userInput" placeholder="请输入消息..." @confirm="sendMessage" />
      <button @click="sendMessage">发送</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      userInput: '', // 输入框内容
      messages: []    // 消息数组
    };
  },
  methods: {
    sendMessage() {
      if (!this.userInput.trim()) return; // 如果输入为空，不发送

      // 用户消息显示在右侧
      this.messages.push({ text: this.userInput, isUser: true });

      // 清空输入框
      const messageToSend = this.userInput;
      this.userInput = '';

      // 调用后端接口
      this.getResponseFromBackend(messageToSend);
    },
    // getResponseFromBackend(userMessage) {
    //   // 调用后端接口
    //   uni.request({
    //     url: 'http://localhost:3000/proxy', // 替换成你的后端接口地址
    //     method: 'POST',
    //     header: {
    //       'Content-Type': 'application/json', // 明确设置请求头
    //     },
    //     data: JSON.stringify({
    //       prompt: userMessage // 确保发送的数据格式为 JSON 字符串
    //     }),
    //     success: (response) => {
    //       console.log("Response from backend:", response); // 调试信息
    //       if (response.statusCode === 200) {
    //         // 假设后端返回字段 `response` 是模型生成的回答
    //         const backendResponse = response.data.response; // 使用 `response` 字段
    //         if (backendResponse) {
    //           this.messages.push({ text: backendResponse, isUser: false });
    //         } else {
    //           this.messages.push({ text: "后端返回的响应格式不正确", isUser: false });
    //         }
    //       } else {
    //         this.messages.push({ text: `错误：${response.statusCode}`, isUser: false });
    //       }
    //     },
    //     fail: (error) => {
    //       console.error("Request failed:", error); // 调试信息
    //       // 请求失败的处理
    //       this.messages.push({ text: "网络错误，请稍后再试", isUser: false });
    //     }
    //   });
    // }
	getResponseFromBackend(userMessage) {
	  // 调用后端代理接口
	  uni.request({
	    url: 'http://127.0.0.1:3000/proxy', // 代理服务器地址
	    method: 'POST',
	    header: {
	      'Content-Type': 'application/json', // 明确设置请求头
	    },
	    data: JSON.stringify({
	      model: "test",  // 模型名称，根据实际情况修改
	      messages: [
	        {
	          role: "user", // 用户消息
	          content: userMessage // 用户发送的消息
	        }
	      ]
	    }),
	    success: (response) => {
	      console.log("Response from backend:", response); // 调试信息
	      if (response.statusCode === 200) {
	        // 假设后端返回字段 `choices` 是模型生成的回答
	        const backendResponse = response.data.choices && response.data.choices[0].message.content;
	        if (backendResponse) {
	          this.messages.push({ text: backendResponse, isUser: false }); // 显示返回的内容
	        } else {
	          this.messages.push({ text: "后端返回的响应格式不正确", isUser: false });
	        }
	      } else {
	        this.messages.push({ text: `错误：${response.statusCode}`, isUser: false });
	      }
	    },
	    fail: (error) => {
	      console.error("Request failed:", error); // 调试信息
	      // 请求失败的处理
	      this.messages.push({ text: "网络错误，请稍后再试", isUser: false });
	    }
	  });
	}

  }
};
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100%;
  /* background-color: aliceblue; */
}

/* 消息区域 */
.chat-box {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
}

/* 每一条消息容器 */
.message-container {
  /* display: flex; */
  align-items: center;
  margin-bottom: 10px;
}

/* 客户端消息容器 */
.message-left {
  display: flex;
  align-items: center;
}

/* 用户消息容器 */
.message-right {
  display: flex;
  align-items: center;
  flex-direction: row-reverse;
  margin-right: 4vw;
}

/* 头像图片 */
.avatar-img {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  overflow: hidden;
  margin: 0 10px;
}

/* 用户消息 */
.user-msg {
  text-align: left;
  background-color: #f0effd;
  color: black;
  padding: 10px;
  border-radius: 10px;
  max-width: 65%;
}

/* 服务端消息 */
.server-msg {
  text-align: left;
  background-color: #f8f8f8;
  color: black;
  padding: 10px;
  border-radius: 10px;
  max-width: 65%;
}

/* 输入框区域，固定在页面底部 */
.input-container {
  display: flex;
  padding: 10px;
  background-color: #f0effd;
  align-items: center;
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  z-index: 10;
}

input {
  flex: 1;
  padding: 5px;
  margin-right: 10px;
  border-radius: 5px;
}

button {
  margin-right: 5vw;
  background-image: linear-gradient(to right, #C088C4, #87B1E0);
  color: white;
  border-radius: 5px;
}
</style>