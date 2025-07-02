<template>
  <div class="stats-container">
    <div class="stats-header">
      <h2 class="stats-title">통계 대시보드</h2>
      <p class="stats-subtitle">실시간 의료 시스템 현황</p>
    </div>
    
    <!-- 주요 지표 카드 -->
    <div class="metrics-grid">
      <div class="metric-card total">
        <div class="metric-icon">👥</div>
        <div class="metric-info">
          <div class="metric-value">{{ stats.totalPatients.toLocaleString() }}</div>
          <div class="metric-label">총 환자 수</div>
          <div class="metric-change positive">+{{ getTodayIncrease() }}명 오늘</div>
        </div>
      </div>
      
      <div class="metric-card emergency">
        <div class="metric-icon">🚨</div>
        <div class="metric-info">
          <div class="metric-value">{{ stats.emergencyCases }}</div>
          <div class="metric-label">응급 환자</div>
          <div class="metric-change">실시간</div>
        </div>
      </div>
      
      <div class="metric-card general">
        <div class="metric-icon">🏥</div>
        <div class="metric-info">
          <div class="metric-value">{{ stats.generalCases.toLocaleString() }}</div>
          <div class="metric-label">일반 진료</div>
          <div class="metric-change positive">{{ getGeneralPercentage() }}%</div>
        </div>
      </div>
      
      <div class="metric-card response">
        <div class="metric-icon">⚡</div>
        <div class="metric-info">
          <div class="metric-value">{{ stats.responseTime }}</div>
          <div class="metric-label">평균 응답시간</div>
          <div class="metric-change positive">-0.2초</div>
        </div>
      </div>
      
      <div class="metric-card satisfaction">
        <div class="metric-icon">⭐</div>
        <div class="metric-info">
          <div class="metric-value">{{ stats.satisfaction }}%</div>
          <div class="metric-label">만족도</div>
          <div class="metric-change positive">+2.3%</div>
        </div>
      </div>
    </div>
    
    <!-- 차트 섹션 -->
    <div class="charts-section">
      <!-- 진료과별 환자 분포 -->
      <div class="chart-container">
        <h3 class="chart-title">진료과별 환자 분포</h3>
        <div class="bar-chart">
          <div 
            v-for="item in departmentData" 
            :key="item.department"
            class="bar-item"
          >
            <div 
              class="bar"
              :style="{ height: `${(item.count / getMaxDepartmentCount()) * 100}%` }"
              :data-count="item.count"
            >
              {{ item.count }}
            </div>
            <div class="bar-label">{{ item.department }}</div>
          </div>
        </div>
      </div>
      
      <!-- 시간대별 상담 현황 -->
      <div class="chart-container">
        <h3 class="chart-title">시간대별 상담 현황</h3>
        <div class="line-chart-container">
          <canvas ref="lineChart" class="line-chart"></canvas>
        </div>
      </div>
      
      <!-- 증상별 통계 -->
      <div class="chart-container">
        <h3 class="chart-title">주요 증상별 통계</h3>
        <div class="symptom-stats">
          <div 
            v-for="symptom in symptomData" 
            :key="symptom.name"
            class="symptom-item"
          >
            <div class="symptom-info">
              <span class="symptom-name">{{ symptom.name }}</span>
              <span class="symptom-count">{{ symptom.count }}건</span>
            </div>
            <div class="symptom-bar">
              <div 
                class="symptom-fill"
                :style="{ 
                  width: `${(symptom.count / getMaxSymptomCount()) * 100}%`,
                  backgroundColor: symptom.color 
                }"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 실시간 활동 -->
    <div class="activity-section">
      <h3 class="section-title">실시간 활동</h3>
      <div class="activity-feed">
        <div 
          v-for="activity in recentActivities" 
          :key="activity.id"
          :class="['activity-item', activity.type]"
        >
          <div class="activity-icon">{{ getActivityIcon(activity.type) }}</div>
          <div class="activity-content">
            <div class="activity-text">{{ activity.message }}</div>
            <div class="activity-time">{{ formatTime(activity.timestamp) }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

const props = defineProps({
  stats: {
    type: Object,
    required: true
  }
})

const lineChart = ref(null)
const recentActivities = ref([
  {
    id: 1,
    type: 'consultation',
    message: '새로운 환자 상담이 시작되었습니다.',
    timestamp: new Date()
  },
  {
    id: 2,
    type: 'appointment',
    message: '진료 예약이 완료되었습니다.',
    timestamp: new Date(Date.now() - 2 * 60 * 1000)
  },
  {
    id: 3,
    type: 'emergency',
    message: '응급 환자가 등록되었습니다.',
    timestamp: new Date(Date.now() - 5 * 60 * 1000)
  }
])

const departmentData = [
  { department: '내과', count: 340 },
  { department: '외과', count: 220 },
  { department: '소아과', count: 180 },
  { department: '정형외과', count: 160 },
  { department: '피부과', count: 120 },
  { department: '안과', count: 100 },
  { department: '이비인후과', count: 90 },
  { department: '신경과', count: 80 }
]

const symptomData = [
  { name: '발열', count: 125, color: '#ef4444' },
  { name: '두통', count: 98, color: '#f59e0b' },
  { name: '기침', count: 87, color: '#10b981' },
  { name: '복통', count: 76, color: '#3b82f6' },
  { name: '피로감', count: 65, color: '#8b5cf6' },
  { name: '어지러움', count: 54, color: '#06b6d4' },
  { name: '관절통', count: 43, color: '#84cc16' },
  { name: '소화불량', count: 32, color: '#f97316' }
]

const getTodayIncrease = () => {
  return Math.floor(Math.random() * 50) + 10
}

const getGeneralPercentage = () => {
  return Math.round((props.stats.generalCases / props.stats.totalPatients) * 100)
}

const getMaxDepartmentCount = () => {
  return Math.max(...departmentData.map(item => item.count))
}

const getMaxSymptomCount = () => {
  return Math.max(...symptomData.map(item => item.count))
}

const getActivityIcon = (type) => {
  const icons = {
    consultation: '💬',
    appointment: '📅',
    emergency: '🚨',
    system: '⚙️'
  }
  return icons[type] || '📋'
}

const formatTime = (timestamp) => {
  const now = new Date()
  const diff = Math.floor((now - timestamp) / 1000 / 60)
  
  if (diff < 1) return '방금 전'
  if (diff < 60) return `${diff}분 전`
  if (diff < 1440) return `${Math.floor(diff / 60)}시간 전`
  return timestamp.toLocaleDateString()
}

const drawLineChart = () => {
  if (!lineChart.value) return
  
  const canvas = lineChart.value
  const ctx = canvas.getContext('2d')
  const width = canvas.width = canvas.offsetWidth
  const height = canvas.height = 200
  
  // 차트 데이터 (시간대별 상담 수)
  const data = [12, 19, 23, 31, 28, 35, 42, 38, 45, 52, 48, 55, 62, 58, 65, 70, 68, 72, 75, 68, 60, 45, 32, 18]
  const maxValue = Math.max(...data)
  
  // 배경 그리기
  ctx.fillStyle = '#f8fafc'
  ctx.fillRect(0, 0, width, height)
  
  // 격자 그리기
  ctx.strokeStyle = '#e2e8f0'
  ctx.lineWidth = 1
  
  // 수평선
  for (let i = 0; i <= 5; i++) {
    const y = (height / 5) * i
    ctx.beginPath()
    ctx.moveTo(0, y)
    ctx.lineTo(width, y)
    ctx.stroke()
  }
  
  // 수직선
  for (let i = 0; i <= 24; i += 4) {
    const x = (width / 24) * i
    ctx.beginPath()
    ctx.moveTo(x, 0)
    ctx.lineTo(x, height)
    ctx.stroke()
  }
  
  // 선 그래프 그리기
  ctx.strokeStyle = '#006ba6'
  ctx.lineWidth = 3
  ctx.beginPath()
  
  data.forEach((value, index) => {
    const x = (width / (data.length - 1)) * index
    const y = height - (value / maxValue) * height
    
    if (index === 0) {
      ctx.moveTo(x, y)
    } else {
      ctx.lineTo(x, y)
    }
  })
  
  ctx.stroke()
  
  // 데이터 포인트 그리기
  ctx.fillStyle = '#006ba6'
  data.forEach((value, index) => {
    const x = (width / (data.length - 1)) * index
    const y = height - (value / maxValue) * height
    
    ctx.beginPath()
    ctx.arc(x, y, 4, 0, Math.PI * 2)
    ctx.fill()
  })
}

const addRandomActivity = () => {
  const activities = [
    { type: 'consultation', message: '새로운 환자 상담이 시작되었습니다.' },
    { type: 'appointment', message: '진료 예약이 완료되었습니다.' },
    { type: 'system', message: '시스템 업데이트가 완료되었습니다.' }
  ]
  
  const randomActivity = activities[Math.floor(Math.random() * activities.length)]
  recentActivities.value.unshift({
    id: Date.now(),
    ...randomActivity,
    timestamp: new Date()
  })
  
  // 최대 10개 활동만 유지
  if (recentActivities.value.length > 10) {
    recentActivities.value = recentActivities.value.slice(0, 10)
  }
}

let activityInterval

onMounted(() => {
  drawLineChart()
  window.addEventListener('resize', drawLineChart)
  
  // 30초마다 새로운 활동 추가
  activityInterval = setInterval(addRandomActivity, 30000)
})

onUnmounted(() => {
  window.removeEventListener('resize', drawLineChart)
  if (activityInterval) {
    clearInterval(activityInterval)
  }
})
</script>

<style scoped>
.stats-container {
  padding: var(--spacing-6);
  background: var(--color-chat-background);
  min-height: 70vh;
  overflow-y: auto;
}

.stats-header {
  text-align: center;
  margin-bottom: var(--spacing-6);
}

.stats-title {
  font-size: var(--font-size-2xl);
  font-weight: 700;
  color: var(--color-medical-primary);
  margin-bottom: var(--spacing-2);
}

.stats-subtitle {
  color: var(--color-gray-600);
  font-size: var(--font-size-base);
}

.metrics-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: var(--spacing-4);
  margin-bottom: var(--spacing-8);
}

.metric-card {
  background: var(--color-white);
  padding: var(--spacing-6);
  border-radius: var(--border-radius-xl);
  box-shadow: var(--shadow-md);
  display: flex;
  align-items: center;
  gap: var(--spacing-4);
  transition: transform var(--transition-fast);
  border-left: 4px solid;
}

.metric-card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
}

.metric-card.total { border-left-color: var(--color-medical-primary); }
.metric-card.emergency { border-left-color: var(--color-medical-error); }
.metric-card.general { border-left-color: var(--color-medical-success); }
.metric-card.response { border-left-color: var(--color-medical-accent); }
.metric-card.satisfaction { border-left-color: var(--color-medical-info); }

.metric-icon {
  font-size: 2.5rem;
  opacity: 0.8;
}

.metric-value {
  font-size: var(--font-size-2xl);
  font-weight: 700;
  color: var(--color-gray-800);
  margin-bottom: var(--spacing-1);
}

.metric-label {
  font-size: var(--font-size-sm);
  color: var(--color-gray-600);
  margin-bottom: var(--spacing-1);
}

.metric-change {
  font-size: var(--font-size-xs);
  padding: var(--spacing-1) var(--spacing-2);
  border-radius: var(--border-radius-sm);
  background: var(--color-gray-100);
  color: var(--color-gray-600);
}

.metric-change.positive {
  background: #dcfce7;
  color: #166534;
}

.charts-section {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: var(--spacing-6);
  margin-bottom: var(--spacing-8);
}

.chart-container {
  background: var(--color-white);
  padding: var(--spacing-6);
  border-radius: var(--border-radius-xl);
  box-shadow: var(--shadow-md);
}

.chart-title {
  font-size: var(--font-size-lg);
  font-weight: 600;
  color: var(--color-gray-800);
  margin-bottom: var(--spacing-4);
  text-align: center;
}

.bar-chart {
  display: flex;
  align-items: end;
  gap: var(--spacing-2);
  height: 200px;
  padding: var(--spacing-4) 0;
}

.bar-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
}

.bar {
  background: linear-gradient(to top, var(--color-medical-primary), var(--color-medical-secondary));
  border-radius: var(--border-radius-sm) var(--border-radius-sm) 0 0;
  width: 100%;
  min-height: 20px;
  display: flex;
  align-items: end;
  justify-content: center;
  color: var(--color-white);
  font-weight: 600;
  font-size: var(--font-size-sm);
  padding: var(--spacing-1);
  transition: all var(--transition-fast);
  position: relative;
}

.bar:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-md);
}

.bar-label {
  margin-top: var(--spacing-2);
  font-size: var(--font-size-xs);
  color: var(--color-gray-600);
  text-align: center;
  word-break: keep-all;
}

.line-chart-container {
  height: 200px;
}

.line-chart {
  width: 100%;
  height: 100%;
  border-radius: var(--border-radius-md);
}

.symptom-stats {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-3);
}

.symptom-item {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-2);
}

.symptom-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.symptom-name {
  font-weight: 600;
  color: var(--color-gray-700);
}

.symptom-count {
  font-size: var(--font-size-sm);
  color: var(--color-gray-500);
}

.symptom-bar {
  height: 8px;
  background: var(--color-gray-200);
  border-radius: var(--border-radius-sm);
  overflow: hidden;
}

.symptom-fill {
  height: 100%;
  border-radius: var(--border-radius-sm);
  transition: width var(--transition-slow);
}

.activity-section {
  background: var(--color-white);
  padding: var(--spacing-6);
  border-radius: var(--border-radius-xl);
  box-shadow: var(--shadow-md);
}

.section-title {
  font-size: var(--font-size-lg);
  font-weight: 600;
  color: var(--color-gray-800);
  margin-bottom: var(--spacing-4);
}

.activity-feed {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-3);
  max-height: 300px;
  overflow-y: auto;
}

.activity-item {
  display: flex;
  align-items: center;
  gap: var(--spacing-3);
  padding: var(--spacing-3);
  border-radius: var(--border-radius-lg);
  transition: background-color var(--transition-fast);
  animation: slideIn 0.3s ease-out;
}

.activity-item:hover {
  background: var(--color-gray-50);
}

.activity-item.emergency {
  background: #fef2f2;
  border: 1px solid #fecaca;
}

.activity-icon {
  font-size: var(--font-size-xl);
  flex-shrink: 0;
}

.activity-text {
  font-size: var(--font-size-sm);
  color: var(--color-gray-700);
}

.activity-time {
  font-size: var(--font-size-xs);
  color: var(--color-gray-500);
  margin-top: var(--spacing-1);
}

@media (max-width: 768px) {
  .stats-container {
    padding: var(--spacing-4);
  }
  
  .metrics-grid {
    grid-template-columns: 1fr;
  }
  
  .charts-section {
    grid-template-columns: 1fr;
  }
  
  .metric-card {
    padding: var(--spacing-4);
  }
  
  .bar-chart {
    gap: var(--spacing-1);
  }
  
  .bar-label {
    font-size: 10px;
  }
}
</style>
