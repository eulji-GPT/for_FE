<template>
  <div class="chat-area">
    <!-- 메시지 영역 -->
    <div class="chat-messages" ref="messagesContainer">
      <div 
        v-for="message in messages" 
        :key="message.id"
        :class="['message', message.type]"
      >
        <div class="message-bubble">
          <div class="message-content" v-html="formatMessage(message.content)"></div>
          <div class="message-time">{{ formatTime(message.timestamp) }}</div>
        </div>
      </div>
      
      <!-- 타이핑 인디케이터 -->
      <div v-if="isTyping" class="message bot">
        <div class="message-bubble typing">
          <div class="typing-dots">
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 빠른 응답 버튼 -->
    <div class="quick-actions">
      <button 
        v-for="action in quickActions" 
        :key="action.id"
        class="quick-action-btn"
        @click="sendQuickMessage(action.message)"
      >
        {{ action.label }}
      </button>
    </div>
    
    <!-- 메시지 입력 영역 -->
    <div class="chat-input-container">
      <input 
        v-model="currentMessage"
        @keyup.enter="handleSendMessage"
        @keydown="handleKeyDown"
        class="chat-input"
        placeholder="메시지를 입력하세요..."
        :disabled="isTyping"
      />
      <button 
        @click="handleSendMessage"
        :disabled="!currentMessage.trim() || isTyping"
        class="send-button"
      >
        {{ isTyping ? '전송중...' : '전송' }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick, watch } from 'vue'

const props = defineProps({
  messages: {
    type: Array,
    default: () => []
  },
  isTyping: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['send-message'])

const currentMessage = ref('')
const messagesContainer = ref(null)

const quickActions = [
  { id: 1, label: '진료 예약', message: '진료 예약을 하고 싶습니다' },
  { id: 2, label: '응급상황', message: '응급상황입니다' },
  { id: 3, label: '증상 문의', message: '증상에 대해 문의하고 싶습니다' },
  { id: 4, label: '진료과 안내', message: '어느 과에 가야 할지 모르겠습니다' }
]

const handleSendMessage = () => {
  if (currentMessage.value.trim() && !props.isTyping) {
    emit('send-message', currentMessage.value.trim())
    currentMessage.value = ''
  }
}

const sendQuickMessage = (message) => {
  emit('send-message', message)
}

const handleKeyDown = (event) => {
  if (event.key === 'Enter' && event.shiftKey) {
    // Shift + Enter로 줄바꿈 허용
    return
  }
}

const formatMessage = (content) => {
  return content.replace(/\n/g, '<br>')
}

const formatTime = (timestamp) => {
  return new Date(timestamp).toLocaleTimeString('ko-KR', {
    hour: '2-digit',
    minute: '2-digit'
  })
}

const scrollToBottom = () => {
  nextTick(() => {
    if (messagesContainer.value) {
      messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight
    }
  })
}

// 메시지가 추가될 때마다 스크롤을 맨 아래로
watch(() => props.messages.length, scrollToBottom)
watch(() => props.isTyping, scrollToBottom)
</script>

<style scoped>
.chat-area {
  display: flex;
  flex-direction: column;
  height: 70vh;
  background: var(--color-chat-background);
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: var(--spacing-6);
  display: flex;
  flex-direction: column;
  gap: var(--spacing-4);
}

.message {
  display: flex;
  animation: slideIn 0.3s ease-out;
}

.message.user {
  justify-content: flex-end;
}

.message.bot {
  justify-content: flex-start;
}

.message.system {
  justify-content: center;
}

.message-bubble {
  max-width: 70%;
  padding: var(--spacing-3) var(--spacing-4);
  border-radius: var(--border-radius-lg);
  position: relative;
}

.message.user .message-bubble {
  background: var(--color-chat-user);
  color: var(--color-white);
  border-bottom-right-radius: var(--border-radius-sm);
}

.message.bot .message-bubble {
  background: var(--color-chat-bot);
  color: var(--color-white);
  border-bottom-left-radius: var(--border-radius-sm);
}

.message.system .message-bubble {
  background: var(--color-gray-100);
  color: var(--color-gray-700);
  border: 1px solid var(--color-gray-200);
  max-width: 80%;
}

.message-content {
  line-height: 1.5;
  word-wrap: break-word;
}

.message-time {
  font-size: var(--font-size-xs);
  opacity: 0.7;
  margin-top: var(--spacing-1);
  text-align: right;
}

.message.bot .message-time {
  text-align: left;
}

.typing-dots {
  display: flex;
  gap: 4px;
  padding: var(--spacing-2) 0;
}

.typing-dots span {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.7);
  animation: typing 1.4s infinite ease-in-out;
}

.typing-dots span:nth-child(1) { animation-delay: 0ms; }
.typing-dots span:nth-child(2) { animation-delay: 200ms; }
.typing-dots span:nth-child(3) { animation-delay: 400ms; }

@keyframes typing {
  0%, 60%, 100% { transform: translateY(0); opacity: 0.5; }
  30% { transform: translateY(-10px); opacity: 1; }
}

.quick-actions {
  display: flex;
  gap: var(--spacing-2);
  padding: var(--spacing-4) var(--spacing-6);
  flex-wrap: wrap;
  border-top: 1px solid var(--color-gray-200);
  background: var(--color-white);
}

.quick-action-btn {
  padding: var(--spacing-2) var(--spacing-4);
  background: var(--color-gray-100);
  border: 1px solid var(--color-gray-300);
  border-radius: var(--border-radius-full);
  cursor: pointer;
  font-size: var(--font-size-sm);
  transition: all var(--transition-fast);
  white-space: nowrap;
}

.quick-action-btn:hover {
  background: var(--color-medical-primary);
  color: var(--color-white);
  transform: translateY(-1px);
}

.chat-input-container {
  display: flex;
  gap: var(--spacing-3);
  padding: var(--spacing-4) var(--spacing-6);
  background: var(--color-white);
  border-top: 1px solid var(--color-gray-200);
}

.chat-input {
  flex: 1;
  padding: var(--spacing-3) var(--spacing-4);
  border: 2px solid var(--color-gray-200);
  border-radius: var(--border-radius-full);
  font-size: var(--font-size-base);
  outline: none;
  transition: border-color var(--transition-fast);
}

.chat-input:focus {
  border-color: var(--color-medical-primary);
}

.chat-input:disabled {
  background: var(--color-gray-100);
  cursor: not-allowed;
}

.send-button {
  padding: var(--spacing-3) var(--spacing-5);
  background: var(--color-medical-primary);
  color: var(--color-white);
  border: none;
  border-radius: var(--border-radius-full);
  cursor: pointer;
  font-weight: 600;
  transition: all var(--transition-fast);
  white-space: nowrap;
}

.send-button:hover:not(:disabled) {
  background: var(--color-medical-secondary);
  transform: translateY(-1px);
}

.send-button:disabled {
  background: var(--color-gray-400);
  cursor: not-allowed;
  transform: none;
}

@keyframes slideIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 768px) {
  .chat-messages {
    padding: var(--spacing-4);
  }
  
  .quick-actions {
    padding: var(--spacing-3);
  }
  
  .chat-input-container {
    padding: var(--spacing-3);
  }
  
  .message-bubble {
    max-width: 85%;
  }
  
  .quick-action-btn {
    font-size: var(--font-size-xs);
    padding: var(--spacing-1) var(--spacing-3);
  }
}
</style>
