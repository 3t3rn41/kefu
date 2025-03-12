<template>
  <div ref="modal" class="custom-modal">
    <a-modal v-model:open="internalOpen" ok-text="确定" cancel-text="取消" @ok="handleOk" :getContainer="() => $refs.modal">
    <div class="settings">
      <h2>预设提示词</h2>
      <a-textarea v-model:value="newPrompts" :rows="5" placeholder="输入预设提示词" />
    </div>
  </a-modal>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';
import { message } from 'ant-design-vue';

const props = defineProps({
  open: {
    type: Boolean,
    required: true,
  },
});

const emit = defineEmits(['update:open', 'update-prompts']);

const newPrompts = ref('');
const internalOpen = ref(props.open);


watch(() => props.open, (newVal) => {
  internalOpen.value = newVal;
});


watch(internalOpen, (newVal) => {
  emit('update:open', newVal);
});

const handleOk = () => {
  if (newPrompts.value) {
    emit('update-prompts', newPrompts.value);
    message.success('预设提示词已更新');
    internalOpen.value = false; 
  } else {
    message.error('不能为空');
  }
};
</script>

<style scoped>
.custom-modal :deep(.ant-modal-content)
{
  background-color: #1a1a1a;
  color: white;
}
.custom-modal :deep(.ant-input)
{
  background-color: #1a1a1a !important;
  color: #ffffff !important;
  border-color: #00ffcc !important;
}
.custom-modal :deep(.ant-input::placeholder) {
  color: gray !important; 
  opacity: 1;
}
</style>