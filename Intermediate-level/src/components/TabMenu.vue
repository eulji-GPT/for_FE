<template>
  <div class="tab-container">
    <nav class="tab-buttons">
      <button 
        v-for="tab in tabs" 
        :key="tab.id"
        :class="['tab-button', { active: activeTab === tab.id }]"
        @click="$emit('tab-change', tab.id)"
      >
        <span class="tab-icon">{{ tab.icon }}</span>
        <span class="tab-label">{{ tab.label }}</span>
      </button>
    </nav>
  </div>
</template>

<script setup>
defineProps({
  activeTab: {
    type: String,
    default: 'chat'
  }
})

defineEmits(['tab-change'])

const tabs = [
  { id: 'chat', label: '챗봇 상담', icon: '💬' },
  { id: 'patient', label: '환자 정보', icon: '👤' },
  { id: 'search', label: '의료 검색', icon: '🔍' },
  { id: 'stats', label: '통계', icon: '📊' }
]
</script>

<style scoped>
.tab-container {
  border-bottom: 2px solid var(--color-gray-200);
  background: var(--color-white);
}

.tab-buttons {
  display: flex;
  gap: 0;
  overflow-x: auto;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.tab-buttons::-webkit-scrollbar {
  display: none;
}

.tab-button {
  padding: var(--spacing-4) var(--spacing-6);
  background: none;
  border: none;
  cursor: pointer;
  font-weight: 600;
  color: var(--color-gray-600);
  border-bottom: 3px solid transparent;
  transition: all var(--transition-normal);
  display: flex;
  align-items: center;
  gap: var(--spacing-2);
  white-space: nowrap;
  flex-shrink: 0;
}

.tab-button:hover {
  color: var(--color-medical-primary);
  background: var(--color-gray-50);
}

.tab-button.active {
  color: var(--color-medical-primary);
  border-bottom-color: var(--color-medical-primary);
  background: var(--color-gray-50);
}

.tab-icon {
  font-size: var(--font-size-lg);
}

.tab-label {
  font-size: var(--font-size-sm);
}

@media (max-width: 768px) {
  .tab-button {
    padding: var(--spacing-3) var(--spacing-4);
  }
  
  .tab-label {
    display: none;
  }
  
  .tab-icon {
    font-size: var(--font-size-xl);
  }
}
</style>
