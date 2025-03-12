<template>
  <a-layout>
    <a-layout-header class="header">
      <h1>智能客服</h1>
    </a-layout-header>
    <div class="chat-container">
      <div class="chat-window">
        <div class="messages">
          <div v-for="(message, index) in messages" :key="index" :class="['message', message.sender]">
            <LoadingOutlined v-if="message.sender=='bot'&&index==messages.length-1&&!isOver" />
            {{ message.text }}
          </div>
        </div>
        <div>
          <a-textarea v-model:value="newMessage" @pressEnter="sendMessage" placeholder="给智能客服发送消息"
            :autoSize="{ minRows: 5, maxRows: 5 }" class="input-box" />
          <span class="send-icon" @click="sendMessage">
            <SendOutlined />
          </span>
        </div>
      </div>
    </div>
  </a-layout>
  <SettingOutlined style="font-size: 28px; margin: 1%; cursor: pointer;" @click="showSetting = true" />
  <a-modal v-model:open="showSetting" ok-text="确定" cancel-text="取消" @ok="handleUpdatePrompts(newPrompts)">
    <div class="settings">
      <h2>预设提示词</h2>
      <a-textarea v-model:value="newPrompts" :rows="5" placeholder="输入预设提示词" />
    </div>
  </a-modal>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { message } from 'ant-design-vue';
import { SettingOutlined, SendOutlined, LoadingOutlined } from '@ant-design/icons-vue';

const messages = ref([]);
const showSetting = ref(false);
const prompts = ref('你是一个智能客服，需要解答用户的疑问');
const newPrompts = ref("");
const newMessage = ref('');
const isOver = ref(false);

const handleSendMessage = (messageText) => {
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
    }, Math.random()*150);
  }, 3000);
};

const sendMessage = () => {
  event.preventDefault();
  if (newMessage.value.trim()) {
    handleSendMessage(newMessage.value.trim());
    isOver.value = false;
  }
  newMessage.value = '';
};

const handleUpdatePrompts = (newPrompts) => {
  if (newPrompts) {
    prompts.value = newPrompts;
    message.success('预设提示词已更新');
    showSetting.value = false;
    prompts.value = newPrompts;
  } else {
    message.error('不能为空');
  }
};

onMounted(() => {
  if (prompts.value != '') {
    newPrompts.value = prompts.value;
  }
});
</script>

<style scoped>
.header {
  background-color: #fff;
  padding: 0 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  z-index: 1;
}

.chat-container {
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 16px;
  height: calc(100vh - 120px);
  overflow-y: auto;
}

.chat-window {
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 50%;
  margin-left: 25%;
}

.messages {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
  
}

.message {
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 8px;
  max-width: 80%;
}
.scroll-container {
  scrollbar-width: none; /* Firefox */
}
.scroll-container::-webkit-scrollbar {
  display: none; /* for Chrome, Safari, and Opera */
}
.message.user {
  background-color: #e6f7ff;
  margin-left: auto;
}

.message.bot {
  background-color: #f0f0f0;
  margin-right: auto;
}

.input-box {
  margin-top: 16px;
  width: 50%;
  margin-left: 25%;
  border: 1px solid gray;
}

.send-icon {
  position: relative;
  bottom: 10%;
  right: 30px;
  cursor: pointer;
  color: #1890ff;
  font-size: 28px;
  transition: color 0.3s;
  z-index: 5;
}

.send-icon:hover {
  color: #40a9ff;
  transform: scale(1.1);
  transition: color 0.3s, transform 0.3s;
}
</style>