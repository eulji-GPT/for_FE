<template>
  <div class="patient-form-container">
    <div class="medical-form">
      <h2 class="form-title">환자 정보 등록</h2>
      <p class="form-subtitle">정확한 진료를 위해 환자 정보를 입력해주세요.</p>
      
      <form @submit.prevent="submitForm" class="patient-form">
        <!-- 기본 정보 -->
        <div class="form-section">
          <h3 class="section-title">기본 정보</h3>
          
          <div class="form-group">
            <label class="form-label" for="name">이름 *</label>
            <input 
              v-model="formData.name"
              id="name"
              type="text" 
              class="form-input"
              placeholder="환자 이름을 입력하세요"
              required
            />
          </div>
          
          <div class="form-row">
            <div class="form-group">
              <label class="form-label" for="birthdate">생년월일 *</label>
              <input 
                v-model="formData.birthdate"
                id="birthdate"
                type="date" 
                class="form-input"
                required
              />
            </div>
            
            <div class="form-group">
              <label class="form-label" for="gender">성별 *</label>
              <select v-model="formData.gender" id="gender" class="form-select" required>
                <option value="">선택하세요</option>
                <option value="male">남성</option>
                <option value="female">여성</option>
              </select>
            </div>
          </div>
          
          <div class="form-group">
            <label class="form-label" for="phone">연락처 *</label>
            <input 
              v-model="formData.phone"
              id="phone"
              type="tel" 
              class="form-input"
              placeholder="010-1234-5678"
              required
            />
          </div>
        </div>
        
        <!-- 증상 정보 -->
        <div class="form-section">
          <h3 class="section-title">증상 정보</h3>
          
          <div class="form-group">
            <label class="form-label">주요 증상 (복수 선택 가능)</label>
            <div class="checkbox-group">
              <label 
                v-for="symptom in symptoms" 
                :key="symptom.id"
                class="checkbox-item"
              >
                <input 
                  v-model="formData.symptoms"
                  type="checkbox" 
                  :value="symptom.id"
                />
                <span>{{ symptom.label }}</span>
              </label>
            </div>
          </div>
          
          <div class="form-group">
            <label class="form-label" for="symptomDetail">증상 상세 설명</label>
            <textarea 
              v-model="formData.symptomDetail"
              id="symptomDetail"
              class="form-textarea"
              placeholder="증상에 대해 자세히 설명해주세요..."
              rows="4"
            ></textarea>
          </div>
          
          <div class="form-row">
            <div class="form-group">
              <label class="form-label" for="severity">심각도</label>
              <select v-model="formData.severity" id="severity" class="form-select">
                <option value="">선택하세요</option>
                <option value="low">경미</option>
                <option value="medium">보통</option>
                <option value="high">심각</option>
                <option value="emergency">응급</option>
              </select>
            </div>
            
            <div class="form-group">
              <label class="form-label" for="duration">증상 지속기간</label>
              <select v-model="formData.duration" id="duration" class="form-select">
                <option value="">선택하세요</option>
                <option value="1day">1일 이내</option>
                <option value="3days">2-3일</option>
                <option value="1week">1주일</option>
                <option value="1month">1개월 이상</option>
              </select>
            </div>
          </div>
        </div>
        
        <!-- 진료과 선택 -->
        <div class="form-section">
          <h3 class="section-title">희망 진료과</h3>
          
          <div class="form-group">
            <label class="form-label" for="department">진료과</label>
            <select v-model="formData.department" id="department" class="form-select">
              <option value="">선택하세요</option>
              <option value="internal">내과</option>
              <option value="surgery">외과</option>
              <option value="pediatrics">소아과</option>
              <option value="orthopedics">정형외과</option>
              <option value="dermatology">피부과</option>
              <option value="ophthalmology">안과</option>
              <option value="ent">이비인후과</option>
              <option value="psychiatry">정신건강의학과</option>
            </select>
          </div>
          
          <div class="form-group">
            <label class="form-label" for="preferredDate">희망 진료일</label>
            <input 
              v-model="formData.preferredDate"
              id="preferredDate"
              type="date" 
              class="form-input"
              :min="today"
            />
          </div>
        </div>
        
        <!-- 추가 정보 -->
        <div class="form-section">
          <h3 class="section-title">추가 정보</h3>
          
          <div class="form-group">
            <label class="checkbox-item">
              <input 
                v-model="formData.hasAllergies"
                type="checkbox"
              />
              <span>알레르기가 있습니다</span>
            </label>
          </div>
          
          <div v-if="formData.hasAllergies" class="form-group">
            <label class="form-label" for="allergies">알레르기 정보</label>
            <textarea 
              v-model="formData.allergies"
              id="allergies"
              class="form-textarea"
              placeholder="알레르기 정보를 입력하세요..."
              rows="2"
            ></textarea>
          </div>
          
          <div class="form-group">
            <label class="form-label" for="medications">현재 복용중인 약물</label>
            <textarea 
              v-model="formData.medications"
              id="medications"
              class="form-textarea"
              placeholder="현재 복용중인 약물이 있다면 입력하세요..."
              rows="2"
            ></textarea>
          </div>
        </div>
        
        <!-- 제출 버튼 -->
        <div class="form-actions">
          <button type="button" @click="resetForm" class="btn-secondary">
            초기화
          </button>
          <button type="submit" class="btn-primary" :disabled="!isFormValid">
            환자 정보 제출
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['submit-form'])

const today = new Date().toISOString().split('T')[0]

const formData = ref({
  name: '',
  birthdate: '',
  gender: '',
  phone: '',
  symptoms: [],
  symptomDetail: '',
  severity: '',
  duration: '',
  department: '',
  preferredDate: '',
  hasAllergies: false,
  allergies: '',
  medications: ''
})

const symptoms = [
  { id: 'fever', label: '발열' },
  { id: 'headache', label: '두통' },
  { id: 'cough', label: '기침' },
  { id: 'sore_throat', label: '목 아픔' },
  { id: 'nausea', label: '구역질' },
  { id: 'diarrhea', label: '설사' },
  { id: 'pain', label: '통증' },
  { id: 'fatigue', label: '피로감' },
  { id: 'dizziness', label: '어지러움' },
  { id: 'shortness_breath', label: '호흡곤란' }
]

const isFormValid = computed(() => {
  return formData.value.name && 
         formData.value.birthdate && 
         formData.value.gender && 
         formData.value.phone
})

const submitForm = () => {
  if (isFormValid.value) {
    emit('submit-form', { ...formData.value })
    resetForm()
  }
}

const resetForm = () => {
  formData.value = {
    name: '',
    birthdate: '',
    gender: '',
    phone: '',
    symptoms: [],
    symptomDetail: '',
    severity: '',
    duration: '',
    department: '',
    preferredDate: '',
    hasAllergies: false,
    allergies: '',
    medications: ''
  }
}
</script>

<style scoped>
.patient-form-container {
  padding: var(--spacing-6);
  background: var(--color-chat-background);
  min-height: 70vh;
  overflow-y: auto;
}

.medical-form {
  background: var(--color-white);
  padding: var(--spacing-8);
  border-radius: var(--border-radius-xl);
  box-shadow: var(--shadow-lg);
  max-width: 800px;
  margin: 0 auto;
}

.form-title {
  font-size: var(--font-size-2xl);
  font-weight: 700;
  color: var(--color-medical-primary);
  margin-bottom: var(--spacing-2);
  text-align: center;
}

.form-subtitle {
  color: var(--color-gray-600);
  text-align: center;
  margin-bottom: var(--spacing-6);
}

.form-section {
  margin-bottom: var(--spacing-8);
  padding-bottom: var(--spacing-6);
  border-bottom: 1px solid var(--color-gray-200);
}

.form-section:last-of-type {
  border-bottom: none;
}

.section-title {
  font-size: var(--font-size-lg);
  font-weight: 600;
  color: var(--color-gray-800);
  margin-bottom: var(--spacing-4);
  padding-bottom: var(--spacing-2);
  border-bottom: 2px solid var(--color-medical-primary);
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--spacing-4);
}

.form-group {
  margin-bottom: var(--spacing-4);
}

.form-label {
  display: block;
  margin-bottom: var(--spacing-2);
  font-weight: 600;
  color: var(--color-gray-700);
  font-size: var(--font-size-sm);
}

.form-input, .form-select, .form-textarea {
  width: 100%;
  padding: var(--spacing-3);
  border: 2px solid var(--color-gray-200);
  border-radius: var(--border-radius-lg);
  font-size: var(--font-size-base);
  transition: border-color var(--transition-fast);
  font-family: inherit;
}

.form-input:focus, .form-select:focus, .form-textarea:focus {
  outline: none;
  border-color: var(--color-medical-primary);
  box-shadow: 0 0 0 3px rgba(0, 107, 166, 0.1);
}

.form-textarea {
  resize: vertical;
  min-height: 100px;
  line-height: 1.5;
}

.checkbox-group {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: var(--spacing-3);
  margin-top: var(--spacing-2);
}

.checkbox-item {
  display: flex;
  align-items: center;
  gap: var(--spacing-2);
  cursor: pointer;
  font-size: var(--font-size-sm);
  padding: var(--spacing-2);
  border-radius: var(--border-radius-md);
  transition: background-color var(--transition-fast);
}

.checkbox-item:hover {
  background: var(--color-gray-50);
}

.checkbox-item input[type="checkbox"] {
  width: 16px;
  height: 16px;
  accent-color: var(--color-medical-primary);
}

.form-actions {
  display: flex;
  gap: var(--spacing-4);
  justify-content: center;
  margin-top: var(--spacing-6);
  padding-top: var(--spacing-6);
  border-top: 1px solid var(--color-gray-200);
}

.btn-primary, .btn-secondary {
  padding: var(--spacing-3) var(--spacing-6);
  border-radius: var(--border-radius-lg);
  font-weight: 600;
  font-size: var(--font-size-base);
  cursor: pointer;
  transition: all var(--transition-fast);
  border: 2px solid;
  min-width: 120px;
}

.btn-primary {
  background: var(--color-medical-primary);
  color: var(--color-white);
  border-color: var(--color-medical-primary);
}

.btn-primary:hover:not(:disabled) {
  background: var(--color-medical-secondary);
  border-color: var(--color-medical-secondary);
  transform: translateY(-2px);
}

.btn-primary:disabled {
  background: var(--color-gray-400);
  border-color: var(--color-gray-400);
  cursor: not-allowed;
  transform: none;
}

.btn-secondary {
  background: transparent;
  color: var(--color-gray-600);
  border-color: var(--color-gray-300);
}

.btn-secondary:hover {
  background: var(--color-gray-100);
  border-color: var(--color-gray-400);
  transform: translateY(-2px);
}

@media (max-width: 768px) {
  .patient-form-container {
    padding: var(--spacing-4);
  }
  
  .medical-form {
    padding: var(--spacing-6);
  }
  
  .form-row {
    grid-template-columns: 1fr;
  }
  
  .checkbox-group {
    grid-template-columns: 1fr;
  }
  
  .form-actions {
    flex-direction: column;
  }
}
</style>
