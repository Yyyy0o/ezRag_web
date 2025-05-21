<script setup>
import { ref } from 'vue';

const question = ref('');
const isLoading = ref(false);
const chatHistory = ref([
  // 初始欢迎消息
  { role: 'assistant', content: '你好！我是AI助手，请问有什么可以帮助你的？' }
]);

// 发送问题到大模型
const sendQuestion = () => {
  if (!question.value.trim()) return;
  
  // 添加用户问题到聊天历史
  chatHistory.value.push({ role: 'user', content: question.value });
  
  // 设置加载状态
  isLoading.value = true;
  
  // 保存当前问题并清空输入框
  const currentQuestion = question.value;
  question.value = '';
  
  // 模拟API调用延迟
  setTimeout(() => {
    // 实际项目中，这里应该是调用大模型API
    // 这里使用模拟响应
    const response = generateMockResponse(currentQuestion);
    
    // 添加AI回复到聊天历史
    chatHistory.value.push({ role: 'assistant', content: response });
    
    // 关闭加载状态
    isLoading.value = false;
  }, 1000);
};

// 模拟大模型回复
const generateMockResponse = (question) => {
  // 简单的关键词匹配模拟
  if (question.includes('你好') || question.includes('嗨') || question.includes('hi')) {
    return '你好！有什么我可以帮助你的吗？';
  } else if (question.includes('文件') || question.includes('上传')) {
    return '您可以在文件管理页面上传和管理您的文件。支持PDF、Word、Excel等多种格式。';
  } else if (question.includes('功能') || question.includes('使用')) {
    return '本系统主要有两个功能：1. 文件管理：上传、查看和删除文件；2. AI问答：基于您的问题提供智能回答。';
  } else {
    return '感谢您的提问。在实际应用中，我会连接到大型语言模型API来提供更准确的回答。您还有其他问题吗？';
  }
};

// 处理按键事件，按Enter发送消息
const handleKeyDown = (event) => {
  if (event.key === 'Enter' && !event.shiftKey) {
    event.preventDefault();
    sendQuestion();
  }
};
</script>

<template>
  <div class="chat-container">
    <h2>AI问答助手</h2>
    
    <!-- 聊天历史记录 -->
    <div class="chat-history">
      <div 
        v-for="(message, index) in chatHistory" 
        :key="index"
        :class="['message', message.role === 'user' ? 'user-message' : 'assistant-message']"
      >
        <div class="message-content">
          {{ message.content }}
        </div>
      </div>
      
      <!-- 加载指示器 -->
      <div v-if="isLoading" class="message assistant-message">
        <div class="message-content loading">
          <span class="dot"></span>
          <span class="dot"></span>
          <span class="dot"></span>
        </div>
      </div>
    </div>
    
    <!-- 输入区域 -->
    <div class="input-area">
      <textarea 
        v-model="question" 
        placeholder="请输入您的问题..."
        @keydown="handleKeyDown"
        :disabled="isLoading"
      ></textarea>
      <button 
        @click="sendQuestion" 
        :disabled="isLoading || !question.trim()"
      >
        发送
      </button>
    </div>
  </div>
</template>

<style scoped>
.chat-container {
  width: 100%;
  height: 100%;
  padding: 20px;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  min-height: 600px;
}

h2 {
  color: #333;
  margin-bottom: 20px;
  text-align: center;
}

.chat-history {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
  background-color: white;
  border-radius: 8px;
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
}

.message {
  max-width: 80%;
  margin-bottom: 15px;
  padding: 10px 15px;
  border-radius: 18px;
  word-break: break-word;
}

.user-message {
  align-self: flex-end;
  background-color: #dcf8c6;
  margin-left: auto;
  border-bottom-right-radius: 5px;
}

.assistant-message {
  align-self: flex-start;
  background-color: #e5e5ea;
  margin-right: auto;
  border-bottom-left-radius: 5px;
}

.message-content {
  line-height: 1.4;
}

.loading .dot {
  display: inline-block;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: #999;
  margin: 0 3px;
  animation: bounce 1.4s infinite ease-in-out both;
}

.loading .dot:nth-child(1) {
  animation-delay: -0.32s;
}

.loading .dot:nth-child(2) {
  animation-delay: -0.16s;
}

@keyframes bounce {
  0%, 80%, 100% { transform: scale(0); }
  40% { transform: scale(1); }
}

.input-area {
  display: flex;
  gap: 10px;
}

textarea {
  flex: 1;
  padding: 12px 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  resize: none;
  height: 60px;
  font-family: inherit;
  font-size: 1em;
  outline: none;
  transition: border-color 0.3s;
}

textarea:focus {
  border-color: #4caf50;
}

button {
  padding: 0 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

button:hover:not(:disabled) {
  background-color: #45a049;
}

button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
</style>