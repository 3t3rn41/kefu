<template>
  <a-layout>
    <a-layout-header class="header">
      <h1>智能客服</h1>
    </a-layout-header>
    <div class="chat-container">
      <ChatWindow :messages="messages" :is-over="isOver" @send-message="handleSendMessage" @update-prompts="handleUpdatePrompts" />
    </div>
  </a-layout>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import ChatWindow from './components/ChatWindow.vue';
import {message} from 'ant-design-vue';

const messages = ref([]);
const prompts = ref('你是一个智能客服，需要解答用户的疑问');
const isOver = ref(true); 

const handleSendMessage = (messageText) => {
  if(!isOver.value)
{
  message.error('请等待回复完成哦')
  return
}
  isOver.value = false;
  messages.value.push({ text: messageText, sender: 'user' });
  let index = 0;
  messages.value.push({ text: '', sender: 'bot' });
  const response = '我是智能客服，有什么问题尽管问我！';
  setTimeout(() => {
    const interval = setInterval(() => {
      if (index < response.length) {
        const lastMessage = messages.value[messages.value.length - 1];
        if (lastMessage.sender === 'bot') {
          lastMessage.text += response[index];
        } else {
          messages.value.push({ text: response[index], sender: 'bot' });
        }
        index++;
      } else {
        clearInterval(interval);
        isOver.value = true; 
      }
    }, Math.random() * 150);
  }, 3000);
};

const handleUpdatePrompts = (newPrompts) => {
  if (newPrompts) {
    prompts.value = newPrompts;
  }
};

onMounted(() => {
  if (prompts.value != '') {
    prompts.value = prompts.value;
  }
});
</script>

<style scoped>
body {
  background-color: #0a0a0a;
  color: #ffffff;
}

.header {
  background-color: #1a1a1a;
  padding: 0 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.5);
  z-index: 1;
  color: #00ffcc;
}

.chat-container {
  background-color: #1a1a1a;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.5);
  padding: 16px;
  height: calc(100vh - 64px);
  overflow-y: auto;
}
.css-dev-only-do-not-override-1p3hq3p .ant-modal
{
  background-color: black;
}
</style>