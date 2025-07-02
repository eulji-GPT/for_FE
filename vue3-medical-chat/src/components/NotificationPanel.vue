<template>
  <div class="notification-panel">
    <transition-group name="notification" tag="div">
      <div 
        v-for="notification in notifications" 
        :key="notification.id"
        :class="['notification', notification.type]"
      >
        <div class="notification-content">
          <div class="notification-icon">{{ getNotificationIcon(notification.type) }}</div>
          <div class="notification-text">
            <div class="notification-message">{{ notification.message }}</div>
            <div class="notification-time">{{ formatTime(notification.timestamp) }}</div>
          </div>
        </div>
        <button 
          @click="$emit('remove-notification', notification.id)"
          class="notification-close"
          aria-label="알림 닫기"
        >
          ×
        </button>
      </div>
    </transition-group>
  </div>
</template>

<script setup>
defineProps({
  notifications: {
    type: Array,
    default: () => []
  }
})

defineEmits(['remove-notification'])

const getNotificationIcon = (type) => {
  const icons = {
    success: '✅',
    error: '❌',
    warning: '⚠️',
    info: 'ℹ️'
  }
  return icons[type] || 'ℹ️'
}

const formatTime = (timestamp) => {
  return new Date(timestamp).toLocaleTimeString('ko-KR', {
    hour: '2-digit',
    minute: '2-digit'
  })
}
</script>

<style scoped>
.notification-panel {
  position: fixed;
  top: var(--spacing-4);
  right: var(--spacing-4);
  width: 350px;
  max-height: 400px;
  overflow-y: auto;
  z-index: 1000;
  pointer-events: none;
}

.notification {
  background: var(--color-white);
  border: 1px solid var(--color-gray-200);
  border-radius: var(--border-radius-lg);
  padding: var(--spacing-4);
  margin-bottom: var(--spacing-3);
  box-shadow: var(--shadow-lg);
  position: relative;
  border-left: 4px solid;
  pointer-events: auto;
  max-width: 100%;
}

.notification.success {
  border-left-color: var(--color-medical-success);
  background: linear-gradient(to right, #ecfdf5, var(--color-white));
}

.notification.error {
  border-left-color: var(--color-medical-error);
  background: linear-gradient(to right, #fef2f2, var(--color-white));
}

.notification.warning {
  border-left-color: var(--color-medical-warning);
  background: linear-gradient(to right, #fffbeb, var(--color-white));
}

.notification.info {
  border-left-color: var(--color-medical-info);
  background: linear-gradient(to right, #eff6ff, var(--color-white));
}

.notification-content {
  display: flex;
  align-items: flex-start;
  gap: var(--spacing-3);
  padding-right: var(--spacing-6);
}

.notification-icon {
  font-size: var(--font-size-lg);
  flex-shrink: 0;
  margin-top: var(--spacing-1);
}

.notification-text {
  flex: 1;
  min-width: 0;
}

.notification-message {
  font-size: var(--font-size-sm);
  color: var(--color-gray-800);
  line-height: 1.5;
  word-wrap: break-word;
  margin-bottom: var(--spacing-1);
}

.notification-time {
  font-size: var(--font-size-xs);
  color: var(--color-gray-500);
}

.notification-close {
  position: absolute;
  top: var(--spacing-2);
  right: var(--spacing-2);
  background: none;
  border: none;
  font-size: var(--font-size-lg);
  cursor: pointer;
  color: var(--color-gray-400);
  width: 24px;
  height: 24px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
}

.notification-close:hover {
  color: var(--color-gray-600);
  background: var(--color-gray-100);
}

/* 애니메이션 */
.notification-enter-active {
  animation: slideInRight 0.3s ease-out;
}

.notification-leave-active {
  animation: slideOutRight 0.3s ease-in;
}

@keyframes slideInRight {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes slideOutRight {
  from {
    transform: translateX(0);
    opacity: 1;
  }
  to {
    transform: translateX(100%);
    opacity: 0;
  }
}

@media (max-width: 768px) {
  .notification-panel {
    width: calc(100vw - var(--spacing-8));
    right: var(--spacing-4);
    left: var(--spacing-4);
  }
  
  .notification {
    padding: var(--spacing-3);
  }
  
  .notification-content {
    gap: var(--spacing-2);
  }
}

/* 스크롤바 스타일링 */
.notification-panel::-webkit-scrollbar {
  width: 4px;
}

.notification-panel::-webkit-scrollbar-track {
  background: transparent;
}

.notification-panel::-webkit-scrollbar-thumb {
  background: var(--color-gray-300);
  border-radius: 2px;
}

.notification-panel::-webkit-scrollbar-thumb:hover {
  background: var(--color-gray-400);
}
</style>
