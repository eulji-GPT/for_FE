<template>
  <div class="medical-search-container">
    <div class="search-header">
      <h2 class="search-title">의료 정보 검색</h2>
      <p class="search-subtitle">증상, 질병, 의료 정보를 검색하세요</p>
    </div>
    
    <!-- 검색 영역 -->
    <div class="search-section">
      <div class="search-input-container">
        <input 
          v-model="searchTerm"
          @keyup.enter="handleSearch"
          @input="showAutocomplete"
          class="search-input"
          placeholder="증상이나 질병명을 입력하세요..."
        />
        <button @click="handleSearch" class="search-button">
          🔍 검색
        </button>
      </div>
      
      <!-- 자동완성 드롭다운 -->
      <div v-if="showSuggestions && suggestions.length" class="autocomplete-dropdown">
        <div 
          v-for="(suggestion, index) in suggestions" 
          :key="index"
          :class="['autocomplete-item', { selected: selectedIndex === index }]"
          @click="selectSuggestion(suggestion)"
          @mouseenter="selectedIndex = index"
        >
          {{ suggestion }}
        </div>
      </div>
    </div>
    
    <!-- 빠른 검색 버튼 -->
    <div class="quick-search">
      <h3 class="quick-search-title">빠른 검색</h3>
      <div class="quick-search-buttons">
        <button 
          v-for="category in quickCategories" 
          :key="category.id"
          @click="searchCategory(category.term)"
          class="quick-search-btn"
        >
          <span class="category-icon">{{ category.icon }}</span>
          <span class="category-label">{{ category.label }}</span>
        </button>
      </div>
    </div>
    
    <!-- 검색 결과 -->
    <div v-if="searchResults.length" class="search-results">
      <h3 class="results-title">검색 결과</h3>
      <div class="results-grid">
        <div 
          v-for="result in searchResults" 
          :key="result.id"
          class="result-card"
        >
          <div class="result-header">
            <h4 class="result-title">{{ result.title }}</h4>
            <span :class="['severity-badge', result.severity]">
              {{ getSeverityLabel(result.severity) }}
            </span>
          </div>
          
          <div class="result-content">
            <p class="result-description">{{ result.description }}</p>
            
            <div class="result-details">
              <div class="detail-item">
                <strong>주요 증상:</strong>
                <span>{{ result.symptoms.join(', ') }}</span>
              </div>
              
              <div class="detail-item">
                <strong>권장 진료과:</strong>
                <span class="department-tag">{{ result.department }}</span>
              </div>
              
              <div v-if="result.treatment" class="detail-item">
                <strong>치료방법:</strong>
                <span>{{ result.treatment }}</span>
              </div>
            </div>
          </div>
          
          <div class="result-actions">
            <button @click="bookAppointment(result.department)" class="btn-appointment">
              진료 예약
            </button>
            <button @click="getMoreInfo(result.id)" class="btn-info">
              자세히 보기
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 검색 결과가 없을 때 -->
    <div v-else-if="hasSearched && !isSearching" class="no-results">
      <div class="no-results-icon">🔍</div>
      <h3>검색 결과가 없습니다</h3>
      <p>다른 검색어로 시도해보세요</p>
    </div>
    
    <!-- 로딩 상태 -->
    <div v-if="isSearching" class="loading-state">
      <div class="loading-spinner"></div>
      <p>검색 중...</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['search'])

const searchTerm = ref('')
const searchResults = ref([])
const isSearching = ref(false)
const hasSearched = ref(false)
const showSuggestions = ref(false)
const selectedIndex = ref(-1)

const quickCategories = [
  { id: 1, label: '감기/독감', term: '감기', icon: '🤧' },
  { id: 2, label: '두통', term: '두통', icon: '🤕' },
  { id: 3, label: '소화불량', term: '소화불량', icon: '😷' },
  { id: 4, label: '알레르기', term: '알레르기', icon: '🤒' },
  { id: 5, label: '관절통', term: '관절통', icon: '🦴' },
  { id: 6, label: '피부질환', term: '피부염', icon: '🧴' }
]

const medicalTerms = [
  '감기', '독감', '두통', '편두통', '복통', '설사', '변비', '발열',
  '기침', '인후통', '알레르기', '천식', '고혈압', '당뇨병', '관절염',
  '피부염', '습진', '여드름', '불면증', '우울증', '불안장애', '소화불량'
]

const suggestions = computed(() => {
  if (!searchTerm.value || searchTerm.value.length < 1) return []
  return medicalTerms.filter(term => 
    term.includes(searchTerm.value)
  ).slice(0, 5)
})

const mockSearchResults = {
  '감기': [
    {
      id: 1,
      title: '감기 (상기도 감염)',
      severity: 'low',
      description: '바이러스에 의한 상부 호흡기 감염으로 가장 흔한 질환입니다.',
      symptoms: ['콧물', '기침', '재채기', '인후통', '미열'],
      department: '내과',
      treatment: '충분한 휴식, 수분 섭취, 증상 완화 약물 복용'
    }
  ],
  '두통': [
    {
      id: 2,
      title: '긴장성 두통',
      severity: 'medium',
      description: '스트레스나 근육 긴장으로 인한 가장 흔한 형태의 두통입니다.',
      symptoms: ['머리 전체의 압박감', '목과 어깨 근육 긴장', '피로감'],
      department: '신경과',
      treatment: '휴식, 스트레스 관리, 진통제 복용'
    },
    {
      id: 3,
      title: '편두통',
      severity: 'high',
      description: '혈관성 두통으로 발작적이고 심한 통증이 특징입니다.',
      symptoms: ['한쪽 머리의 심한 통증', '구역질', '빛 공포증', '소리 공포증'],
      department: '신경과',
      treatment: '전문의 진료, 편두통 예방약, 생활습관 개선'
    }
  ],
  '소화불량': [
    {
      id: 4,
      title: '기능성 소화불량',
      severity: 'medium',
      description: '특별한 원인 없이 발생하는 소화 장애 증상입니다.',
      symptoms: ['상복부 불편감', '조기 포만감', '복부 팽만', '구역질'],
      department: '소화기내과',
      treatment: '식이요법, 생활습관 개선, 위장관 운동 촉진제'
    }
  ]
}

const handleSearch = () => {
  if (!searchTerm.value.trim()) return
  
  isSearching.value = true
  hasSearched.value = true
  showSuggestions.value = false
  
  emit('search', searchTerm.value)
  
  // 모의 검색 결과
  setTimeout(() => {
    const results = mockSearchResults[searchTerm.value] || []
    searchResults.value = results
    isSearching.value = false
  }, 1000)
}

const searchCategory = (term) => {
  searchTerm.value = term
  handleSearch()
}

const showAutocomplete = () => {
  showSuggestions.value = true
  selectedIndex.value = -1
}

const selectSuggestion = (suggestion) => {
  searchTerm.value = suggestion
  showSuggestions.value = false
  handleSearch()
}

const getSeverityLabel = (severity) => {
  const labels = {
    low: '경미',
    medium: '보통',
    high: '주의',
    emergency: '응급'
  }
  return labels[severity] || '알 수 없음'
}

const bookAppointment = (department) => {
  alert(`${department} 진료 예약을 진행합니다.`)
}

const getMoreInfo = (id) => {
  alert(`상세 정보를 확인합니다. (ID: ${id})`)
}
</script>

<style scoped>
.medical-search-container {
  padding: var(--spacing-6);
  background: var(--color-chat-background);
  min-height: 70vh;
  overflow-y: auto;
}

.search-header {
  text-align: center;
  margin-bottom: var(--spacing-6);
}

.search-title {
  font-size: var(--font-size-2xl);
  font-weight: 700;
  color: var(--color-medical-primary);
  margin-bottom: var(--spacing-2);
}

.search-subtitle {
  color: var(--color-gray-600);
  font-size: var(--font-size-base);
}

.search-section {
  position: relative;
  margin-bottom: var(--spacing-6);
}

.search-input-container {
  display: flex;
  gap: var(--spacing-3);
  max-width: 600px;
  margin: 0 auto;
}

.search-input {
  flex: 1;
  padding: var(--spacing-4);
  border: 2px solid var(--color-gray-200);
  border-radius: var(--border-radius-lg);
  font-size: var(--font-size-base);
  outline: none;
  transition: border-color var(--transition-fast);
}

.search-input:focus {
  border-color: var(--color-medical-primary);
  box-shadow: 0 0 0 3px rgba(0, 107, 166, 0.1);
}

.search-button {
  padding: var(--spacing-4) var(--spacing-6);
  background: var(--color-medical-primary);
  color: var(--color-white);
  border: none;
  border-radius: var(--border-radius-lg);
  cursor: pointer;
  font-weight: 600;
  transition: all var(--transition-fast);
  white-space: nowrap;
}

.search-button:hover {
  background: var(--color-medical-secondary);
  transform: translateY(-2px);
}

.autocomplete-dropdown {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 600px;
  background: var(--color-white);
  border: 1px solid var(--color-gray-200);
  border-radius: var(--border-radius-lg);
  box-shadow: var(--shadow-lg);
  z-index: 1000;
  max-height: 200px;
  overflow-y: auto;
}

.autocomplete-item {
  padding: var(--spacing-3) var(--spacing-4);
  cursor: pointer;
  transition: background-color var(--transition-fast);
  border-bottom: 1px solid var(--color-gray-100);
}

.autocomplete-item:hover,
.autocomplete-item.selected {
  background: var(--color-gray-50);
}

.autocomplete-item:last-child {
  border-bottom: none;
}

.quick-search {
  margin-bottom: var(--spacing-8);
}

.quick-search-title {
  font-size: var(--font-size-lg);
  font-weight: 600;
  color: var(--color-gray-800);
  margin-bottom: var(--spacing-4);
  text-align: center;
}

.quick-search-buttons {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: var(--spacing-3);
  max-width: 800px;
  margin: 0 auto;
}

.quick-search-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-2);
  padding: var(--spacing-4);
  background: var(--color-white);
  border: 2px solid var(--color-gray-200);
  border-radius: var(--border-radius-lg);
  cursor: pointer;
  transition: all var(--transition-fast);
}

.quick-search-btn:hover {
  border-color: var(--color-medical-primary);
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.category-icon {
  font-size: var(--font-size-2xl);
}

.category-label {
  font-size: var(--font-size-sm);
  font-weight: 600;
  color: var(--color-gray-700);
}

.search-results {
  margin-top: var(--spacing-6);
}

.results-title {
  font-size: var(--font-size-xl);
  font-weight: 600;
  color: var(--color-gray-800);
  margin-bottom: var(--spacing-4);
}

.results-grid {
  display: grid;
  gap: var(--spacing-4);
}

.result-card {
  background: var(--color-white);
  border-radius: var(--border-radius-xl);
  padding: var(--spacing-6);
  box-shadow: var(--shadow-md);
  transition: transform var(--transition-fast);
}

.result-card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
}

.result-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: var(--spacing-4);
}

.result-title {
  font-size: var(--font-size-lg);
  font-weight: 600;
  color: var(--color-gray-800);
}

.severity-badge {
  padding: var(--spacing-1) var(--spacing-3);
  border-radius: var(--border-radius-full);
  font-size: var(--font-size-xs);
  font-weight: 600;
  text-transform: uppercase;
}

.severity-badge.low {
  background: #dcfce7;
  color: #166534;
}

.severity-badge.medium {
  background: #fef3c7;
  color: #92400e;
}

.severity-badge.high {
  background: #fee2e2;
  color: #991b1b;
}

.severity-badge.emergency {
  background: #fecaca;
  color: #7f1d1d;
}

.result-description {
  color: var(--color-gray-600);
  line-height: 1.6;
  margin-bottom: var(--spacing-4);
}

.result-details {
  margin-bottom: var(--spacing-4);
}

.detail-item {
  margin-bottom: var(--spacing-2);
  font-size: var(--font-size-sm);
}

.detail-item strong {
  color: var(--color-gray-700);
  margin-right: var(--spacing-2);
}

.department-tag {
  background: var(--color-medical-primary);
  color: var(--color-white);
  padding: var(--spacing-1) var(--spacing-2);
  border-radius: var(--border-radius-sm);
  font-size: var(--font-size-xs);
  font-weight: 600;
}

.result-actions {
  display: flex;
  gap: var(--spacing-3);
}

.btn-appointment, .btn-info {
  padding: var(--spacing-2) var(--spacing-4);
  border-radius: var(--border-radius-md);
  font-size: var(--font-size-sm);
  font-weight: 600;
  cursor: pointer;
  transition: all var(--transition-fast);
  border: 2px solid;
}

.btn-appointment {
  background: var(--color-medical-primary);
  color: var(--color-white);
  border-color: var(--color-medical-primary);
}

.btn-appointment:hover {
  background: var(--color-medical-secondary);
  border-color: var(--color-medical-secondary);
}

.btn-info {
  background: transparent;
  color: var(--color-gray-600);
  border-color: var(--color-gray-300);
}

.btn-info:hover {
  background: var(--color-gray-100);
  border-color: var(--color-gray-400);
}

.no-results {
  text-align: center;
  padding: var(--spacing-8);
  color: var(--color-gray-500);
}

.no-results-icon {
  font-size: 4rem;
  margin-bottom: var(--spacing-4);
}

.loading-state {
  text-align: center;
  padding: var(--spacing-8);
}

.loading-spinner {
  width: 40px;
  height: 40px;
  border: 4px solid var(--color-gray-200);
  border-top: 4px solid var(--color-medical-primary);
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto var(--spacing-4);
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

@media (max-width: 768px) {
  .medical-search-container {
    padding: var(--spacing-4);
  }
  
  .search-input-container {
    flex-direction: column;
  }
  
  .quick-search-buttons {
    grid-template-columns: repeat(2, 1fr);
  }
  
  .result-actions {
    flex-direction: column;
  }
}
</style>
