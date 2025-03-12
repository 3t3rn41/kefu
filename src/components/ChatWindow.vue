<template>
    <div class="chat-window">
      <div class="messages">
        <div v-for="(message, index) in messages" :key="index" :class="['message', message.sender]">
          <LoadingOutlined v-if="message.sender == 'bot' && index == messages.length - 1 && !isOver" />
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
      <SettingOutlined style="font-size: 28px; bottom: 1%;left: 1%; cursor: pointer;position: absolute;color: white;" @click="showSetting = true" />
      <Preprompt v-model:open="showSetting" @update-prompts="handleUpdatePrompts" />
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { SendOutlined, LoadingOutlined, SettingOutlined } from '@ant-design/icons-vue';
  import Preprompt from './Preprompt.vue';
  
  const props = defineProps({
    messages: {
      type: Array,
      required: true,
    },
    isOver: {
      type: Boolean,
      required: true,
    },
  });
  
  const emit = defineEmits(['send-message', 'update-prompts']);
  
  const newMessage = ref('');
  const showSetting = ref(false);
  
  const sendMessage = () => {
    event.preventDefault();
    if (newMessage.value.trim()) {
      emit('send-message', newMessage.value.trim());
    }
    newMessage.value = '';
  };
  
  const handleUpdatePrompts = (newPrompts) => {
    emit('update-prompts', newPrompts);
  };
  </script>
  
  <style scoped>
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
    color: #ffffff;
  }
  
  .message.user {
    background-color: #003366;
    margin-left: auto;
  }
  
  .message.bot {
    background-color: #004d4d;
    margin-right: auto;
  }
  
  .input-box {
    margin-top: 16px;
    width: 50%;
    margin-left: 25%;
    border: 1px solid #00ffcc;
    background-color: #1a1a1a;
    color: #ffffff;
  }
  
  .input-box::placeholder {
    color: #666666;
  }
  
  .send-icon {
    position: relative;
    bottom: 10%;
    right: 30px;
    cursor: pointer;
    color: #00ffcc;
    font-size: 28px;
    transition: color 0.3s;
    z-index: 5;
  }
  
  .send-icon:hover {
    color: #00cc99;
    transform: scale(1.1);
    transition: color 0.3s, transform 0.3s;
  }
  </style>