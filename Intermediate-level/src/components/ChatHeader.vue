<template>
  <header class="chatbot-header">
    <div class="header-content">
      <h1 class="chatbot-title">🏥 의료IT 챗봇</h1>
      <p class="chatbot-subtitle">24시간 의료상담 및 진료예약 서비스</p>
      <div class="chatbot-status">
        <div class="status-indicator" :class="{ online: isOnline }"></div>
        <span>{{ isOnline ? '온라인' : '오프라인' }}</span>
        <span class="consultation-count">• 총 {{ consultationCount }}건 상담</span>
      </div>
    </div>
  </header>
</template>

<script setup>
defineProps({
  isOnline: {
    type: Boolean,
    default: true
  },
  consultationCount: {
    type: Number,
    default: 0
  }
})
</script>

<style scoped>
.chatbot-header {
  background: linear-gradient(135deg, var(--color-medical-primary), var(--color-medical-secondary));
  color: var(--color-white);
  padding: var(--spacing-6);
  text-align: center;
  position: relative;
}

.chatbot-header::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, var(--color-medical-accent), var(--color-medical-success));
}

.chatbot-title {
  font-size: var(--font-size-2xl);
  font-weight: 700;
  margin-bottom: var(--spacing-2);
  text-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.chatbot-subtitle {
  font-size: var(--font-size-base);
  opacity: 0.9;
  margin-bottom: var(--spacing-4);
}

.chatbot-status {
  display: inline-flex;
  align-items: center;
  gap: var(--spacing-2);
  font-size: var(--font-size-sm);
  background: rgba(255, 255, 255, 0.2);
  padding: var(--spacing-2) var(--spacing-4);
  border-radius: var(--border-radius-full);
  backdrop-filter: blur(10px);
}

.status-indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--color-medical-error);
  animation: pulse 2s infinite;
}

.status-indicator.online {
  background: var(--color-medical-success);
}

.consultation-count {
  font-weight: 600;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
</style>
