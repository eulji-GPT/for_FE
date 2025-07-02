<template>
  <div id="app">
    <div class="container">
      <!-- 챗봇 컨테이너 -->
      <div class="chatbot-container">
        <!-- 챗봇 헤더 -->
        <ChatHeader 
          :isOnline="isOnline"
          :consultationCount="consultationCount"
        />
        
        <!-- 메인 콘텐츠 영역 -->
        <div class="main-content">
          <!-- 탭 메뉴 -->
          <TabMenu 
            :activeTab="activeTab"
            @tab-change="switchTab"
          />
          
          <!-- 챗봇 탭 -->
          <ChatArea 
            v-if="activeTab === 'chat'"
            :messages="messages"
            :isTyping="isTyping"
            @send-message="sendMessage"
          />
          
          <!-- 환자 정보 탭 -->
          <PatientForm 
            v-if="activeTab === 'patient'"
            @submit-form="submitPatientForm"
          />
          
          <!-- 의료 정보 검색 탭 -->
          <MedicalSearch 
            v-if="activeTab === 'search'"
            @search="searchMedicalInfo"
          />
          
          <!-- 통계 대시보드 탭 -->
          <StatsDashboard 
            v-if="activeTab === 'stats'"
            :stats="stats"
          />
        </div>
      </div>
    </div>
    
    <!-- 알림 시스템 -->
    <NotificationPanel 
      :notifications="notifications"
      @remove-notification="removeNotification"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ChatHeader from './components/ChatHeader.vue'
import TabMenu from './components/TabMenu.vue'
import ChatArea from './components/ChatArea.vue'
import PatientForm from './components/PatientForm.vue'
import MedicalSearch from './components/MedicalSearch.vue'
import StatsDashboard from './components/StatsDashboard.vue'
import NotificationPanel from './components/NotificationPanel.vue'

// 반응성 데이터
const activeTab = ref('chat')
const isOnline = ref(true)
const isTyping = ref(false)
const consultationCount = ref(127)
const messages = ref([
  {
    id: 1,
    type: 'system',
    content: '안녕하세요! 의료IT 챗봇입니다. 어떤 도움이 필요하신가요?',
    timestamp: new Date()
  }
])
const notifications = ref([])
const stats = ref({
  totalPatients: 1248,
  emergencyCases: 23,
  generalCases: 1225,
  responseTime: '2.3초',
  satisfaction: 94.5
})

// 메서드
const switchTab = (tabId) => {
  activeTab.value = tabId
}

const sendMessage = async (message) => {
  // 사용자 메시지 추가
  messages.value.push({
    id: Date.now(),
    type: 'user',
    content: message,
    timestamp: new Date()
  })
  
  // 타이핑 인디케이터 표시
  isTyping.value = true
  
  // 봇 응답 시뮬레이션
  setTimeout(() => {
    const botResponse = generateBotResponse(message)
    messages.value.push({
      id: Date.now() + 1,
      type: 'bot',
      content: botResponse,
      timestamp: new Date()
    })
    isTyping.value = false
    consultationCount.value++
  }, 1500)
}

const generateBotResponse = (userMessage) => {
  const message = userMessage.toLowerCase()
  
  if (message.includes('응급') || message.includes('위급')) {
    return '🚨 응급상황이 의심됩니다. 즉시 119에 신고하거나 가까운 응급실로 내원하세요. 응급실 위치를 안내해드릴까요?'
  } else if (message.includes('두통') || message.includes('머리')) {
    return '두통에 대해 문의하셨네요. 두통의 원인은 다양할 수 있습니다:\n\n• 긴장성 두통\n• 편두통\n• 스트레스성 두통\n\n신경과 진료를 권장드리며, 심한 경우 즉시 병원 방문을 권합니다.'
  } else if (message.includes('예약')) {
    return '진료 예약을 도와드리겠습니다. 어느 과를 희망하시나요?\n\n주요 진료과:\n• 내과 (일반진료)\n• 외과 (수술관련)\n• 소아과 (소아진료)\n• 정형외과 (근골격계)\n• 피부과 (피부질환)'
  } else {
    return '문의해주신 내용을 검토 중입니다. 보다 정확한 진단을 위해서는 전문의 상담을 받으시기 바랍니다. 추가 질문이 있으시면 언제든 말씀해주세요.'
  }
}

const submitPatientForm = (formData) => {
  addNotification('success', '환자 정보가 성공적으로 제출되었습니다.')
  console.log('Patient form submitted:', formData)
}

const searchMedicalInfo = (searchTerm) => {
  addNotification('info', `"${searchTerm}"에 대한 의료정보를 검색 중입니다.`)
  console.log('Medical search:', searchTerm)
}

const addNotification = (type, message) => {
  const notification = {
    id: Date.now(),
    type,
    message,
    timestamp: new Date()
  }
  notifications.value.push(notification)
  
  // 5초 후 자동 제거
  setTimeout(() => {
    removeNotification(notification.id)
  }, 5000)
}

const removeNotification = (id) => {
  const index = notifications.value.findIndex(n => n.id === id)
  if (index > -1) {
    notifications.value.splice(index, 1)
  }
}

// 실시간 데이터 업데이트
const updateRealTimeData = () => {
  stats.value.totalPatients += Math.floor(Math.random() * 3)
  if (Math.random() > 0.9) {
    stats.value.emergencyCases += 1
  }
  stats.value.generalCases = stats.value.totalPatients - stats.value.emergencyCases
}

onMounted(() => {
  // 실시간 데이터 업데이트 (5초마다)
  setInterval(updateRealTimeData, 5000)
  
  // 랜덤 알림 (30초마다)
  setInterval(() => {
    const messages = [
      { type: 'info', text: '새로운 환자가 등록되었습니다.' },
      { type: 'warning', text: '시스템 점검이 예정되어 있습니다.' },
      { type: 'success', text: '진료 예약이 완료되었습니다.' }
    ]
    const randomMsg = messages[Math.floor(Math.random() * messages.length)]
    addNotification(randomMsg.type, randomMsg.text)
  }, 30000)
})
</script>

<style scoped>
.chatbot-container {
  background: var(--color-white);
  border-radius: var(--border-radius-2xl);
  box-shadow: var(--shadow-2xl);
  overflow: hidden;
  max-width: 1200px;
  margin: 0 auto;
  min-height: 85vh;
  display: flex;
  flex-direction: column;
}

.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

@media (max-width: 768px) {
  .chatbot-container {
    margin: var(--spacing-4);
    min-height: 90vh;
  }
}
</style>
