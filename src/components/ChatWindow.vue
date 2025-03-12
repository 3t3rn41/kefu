<template>
    <div class="chat-window">

        <div class="messages">
            <div v-for="(message, index) in messages" :key="index" :class="['message', message.sender]">
                {{ message.text }}
            </div>
        </div>


        <div>
            <a-textarea v-model:value="newMessage" @pressEnter="sendMessage" placeholder="给智能客服发送消息"
                :autoSize="{ minRows: 5, maxRows: 5 }"
                class="input-box" />
            <span class="send-icon" @click="sendMessage">
                <SendOutlined />
            </span>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { SendOutlined } from '@ant-design/icons-vue'; 

const props = defineProps({
    messages: {
        type: Array,
        required: true,
    },
});

const emit = defineEmits(['send-message']);

const newMessage = ref('');


const sendMessage = () => {
    event.preventDefault();
    if (newMessage.value.trim()) {
        emit('send-message', newMessage.value.trim());
    }
    newMessage.value = '';
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