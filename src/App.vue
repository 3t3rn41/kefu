<template>
  <a-layout>
    <a-layout-header class="header">
      <h1>智能客服</h1>

    </a-layout-header>
    <div class="chat-container">
      <ChatWindow :messages="messages" @send-message="handleSendMessage" />
    </div>
  </a-layout>
  <SettingOutlined style="font-size: 28px;margin: 1%;cursor: pointer;" @click="showSetting = true" />
  <a-modal v-model:open="showSetting" ok-text="确定" cancel-text="取消" @ok="handleUpdatePrompts(newPrompts)">
    <div class="settings">
      <h2>预设提示词</h2>
      <a-textarea v-model:value="newPrompts" :rows="5" placeholder="输入预设提示词" />
    </div>
  </a-modal>
</template>

<script setup>
import { ref,onMounted} from 'vue';
import ChatWindow from './components/ChatWindow.vue';
import { message } from 'ant-design-vue';
import { SettingOutlined } from '@ant-design/icons-vue'; 

const messages = ref([]);
let showSetting = ref(false)

const prompts = ref('你是一个智能客服，需要解答用户的疑问');
let newPrompts = ref("")

const handleSendMessage = (message) => {
  messages.value.push({ text: message, sender: 'user' });

  const response = '我是智能客服，有什么问题尽管问我！';
  let index = 0;
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
    }
  }, 100); 
};


const handleUpdatePrompts = (newPrompts) => {
  if (newPrompts) {
    prompts.value = newPrompts;
    message.success('预设提示词已更新')
    showSetting.value = false
    prompts.value= newPrompts
  }
  else
  {
    message.error('不能为空')
  }
};
onMounted(() => {
  if(prompts.value != '')
      newPrompts.value = prompts.value
})
</script>

<style scoped>
.header {
  background-color: #fff;
  padding: 0 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  z-index: 1;
}

.content {
  padding: 24px;
  background-color: #f0f2f5;
  min-height: calc(100vh - 64px);
}

.chat-container {
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 16px;
  height: calc(100vh - 120px);
  overflow-y: auto;
}

.settings-container {
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 16px;
  height: calc(100vh - 120px);
  overflow-y: auto;
}
</style>