<script setup>
import { ref, computed, reactive, onMounted } from 'vue'

const tours = [
  { id: 1, title: 'Вокруг острова Елены', basePrice: 6000 },
  { id: 2, title: 'Под Золотой мост', basePrice: 6000 },
  { id: 3, title: 'Под Русский мост', basePrice: 6000 }
]

const generateTimeSlots = () => {
  const slots = []
  for (let hour = 8; hour <= 20; hour++) {
    slots.push(`${hour}:00`)
    if (hour < 20) slots.push(`${hour}:30`)
  }
  return slots
}

const timeSlots = generateTimeSlots()

const form = reactive({
  name: '',
  phone: '+7 ',
  selectedTour: tours[0].id,
  peopleCount: 1,
  message: '',
  date: '',
  time: ''
})

const errors = reactive({
  name: '',
  phone: '',
  peopleCount: '',
  date: '',
  time: ''
})

const showErrors = reactive({
  name: false,
  phone: false,
  peopleCount: false,
  date: false,
  time: false
})

const isSubmitting = ref(false)
const submitSuccess = ref(false)
const animated = ref(false)

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        animated.value = true
        observer.unobserve(entry.target)
      }
    })
  }, { threshold: 0.1 })

  const section = document.querySelector('.contact-section')
  if (section) observer.observe(section)
})

const availableDates = computed(() => {
  const dates = []
  const today = new Date()
  const endDate = new Date()
  endDate.setDate(today.getDate() + 30)
  
  for (let d = new Date(today); d <= endDate; d.setDate(d.getDate() + 1)) {
    dates.push(new Date(d))
  }
  
  return dates
})

const formatDate = (date) => {
  return date.toLocaleDateString('ru-RU', {
    weekday: 'short',
    day: 'numeric',
    month: 'long'
  })
}

const formatDateForInput = (date) => {
  const year = date.getFullYear()
  const month = String(date.getMonth() + 1).padStart(2, '0')
  const day = String(date.getDate()).padStart(2, '0')
  return `${year}-${month}-${day}`
}

const availableTimeSlots = computed(() => {
  if (!form.date) return timeSlots
  
  const selectedDate = new Date(form.date)
  const today = new Date()
  
  if (
    selectedDate.getDate() === today.getDate() &&
    selectedDate.getMonth() === today.getMonth() &&
    selectedDate.getFullYear() === today.getFullYear()
  ) {
    const currentHour = today.getHours()
    const currentMinutes = today.getMinutes()
    
    return timeSlots.filter(slot => {
      const [hour, minute] = slot.split(':').map(Number)
      return hour > currentHour || (hour === currentHour && minute >= currentMinutes)
    })
  }
  
  return timeSlots
})

const totalPrice = computed(() => {
  const selectedTour = tours.find(t => t.id === form.selectedTour)
  const basePrice = selectedTour?.basePrice || 6000
  const people = form.peopleCount
  
  if (people <= 2) return basePrice
  return basePrice + (people - 2) * 1000
})

const hideError = (field) => {
  showErrors[field] = false
}

const handleNameInput = (e) => {
  form.name = e.target.value.replace(/\s+/g, ' ')
  validateName()
}

const formatRussianPhone = (e) => {
  let input = e.target.value.replace(/\D/g, '')
  if (input.length > 11) input = input.substring(0, 11)
  
  let formatted = ''
  if (input.length > 0) {
    formatted = '+7'
    if (input.length > 1) {
      formatted += ' (' + input.substring(1, 4)
    }
    if (input.length > 4) {
      formatted += ') ' + input.substring(4, 7)
    }
    if (input.length > 7) {
      formatted += '-' + input.substring(7, 9)
    }
    if (input.length > 9) {
      formatted += '-' + input.substring(9, 11)
    }
  }
  
  form.phone = formatted
  validatePhone()
}

const validateName = () => {
  const nameRegex = /^[a-zA-Zа-яА-ЯёЁ\s'-]+$/
  showErrors.name = true
  
  if (!form.name.trim()) {
    errors.name = 'Имя обязательно'
  } else if (form.name.length > 50) {
    errors.name = 'Имя слишком длинное'
  } else if (!nameRegex.test(form.name)) {
    errors.name = 'Только буквы и дефисы'
  } else if (form.name.length < 2) {
    errors.name = 'Имя слишком короткое'
  } else {
    errors.name = ''
  }
}

const validatePhone = () => {
  const digitsOnly = form.phone.replace(/\D/g, '')
  showErrors.phone = true
  
  if (!digitsOnly) {
    errors.phone = 'Телефон обязателен'
  } else if (digitsOnly.length !== 11 || !digitsOnly.startsWith('7')) {
    errors.phone = 'Введите корректный номер (+7 XXX XXX-XX-XX)'
  } else {
    errors.phone = ''
  }
}

const validatePeopleCount = () => {
  showErrors.peopleCount = true
  
  if (!form.peopleCount) {
    errors.peopleCount = 'Укажите количество'
  } else if (form.peopleCount < 1) {
    errors.peopleCount = 'Минимум 1 человек'
  } else if (form.peopleCount > 7) {
    errors.peopleCount = 'Максимум 7 человек'
  } else {
    errors.peopleCount = ''
  }
}

const validateDate = () => {
  showErrors.date = true
  
  if (!form.date) {
    errors.date = 'Выберите дату'
  } else {
    errors.date = ''
  }
}

const validateTime = () => {
  showErrors.time = true
  
  if (!form.time) {
    errors.time = 'Выберите время'
  } else {
    errors.time = ''
  }
}

const isFormValid = computed(() => {
  return (
    form.name.trim().length >= 2 &&
    form.phone.replace(/\D/g, '').length === 11 &&
    form.peopleCount >= 1 && form.peopleCount <= 7 &&
    form.date &&
    form.time &&
    !errors.name &&
    !errors.phone &&
    !errors.peopleCount &&
    !errors.date &&
    !errors.time
  )
})

const submitForm = () => {
  validateName()
  validatePhone()
  validatePeopleCount()
  validateDate()
  validateTime()
  
  if (isFormValid.value) {
    isSubmitting.value = true
    
    setTimeout(() => {
      isSubmitting.value = false
      submitSuccess.value = true
      
      const formData = { 
        ...form, 
        totalPrice: totalPrice.value,
        phone: form.phone.replace(/\D/g, ''),
        formattedDate: formatSelectedDate()
      }
      console.log('Form submitted:', formData)
      
      form.name = ''
      form.phone = '+7 '
      form.selectedTour = tours[0].id
      form.peopleCount = 1
      form.message = ''
      form.date = ''
      form.time = ''
      
      Object.keys(showErrors).forEach(key => showErrors[key] = false)
      
      setTimeout(() => {
        submitSuccess.value = false
      }, 5000)
    }, 1500)
  }
}

const formatSelectedDate = () => {
  if (!form.date || !form.time) return ''
  
  const date = new Date(form.date)
  const [hours, minutes] = form.time.split(':').map(Number)
  date.setHours(hours, minutes)
  
  return date.toLocaleString('ru-RU', {
    weekday: 'long',
    day: 'numeric',
    month: 'long',
    hour: '2-digit',
    minute: '2-digit'
  })
}

const contactViaTelegram = () => {
  window.open('https://t.me/Freeman19811', '_blank')
}
</script>

<template>
  <section id="contact" class="contact-section" :class="{ 'animate-in': animated }">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">Забронируйте уже сейчас</h2>
        <div class="section-divider"></div>
      </div>
      
      <div class="contact-container">
        <form @submit.prevent="submitForm" class="contact-form">
          <div class="form-group">
            <label for="name">Ваше имя *</label>
            <div class="input-wrapper">
              <Icon icon="mdi:account-outline" class="input-icon" />
              <input 
                type="text" 
                id="name" 
                v-model="form.name" 
                @input="handleNameInput"
                @focus="hideError('name')"
                @blur="validateName"
                :class="{ 'invalid': errors.name }"
                placeholder="Иван Иванов"
                class="form-input"
              >
            </div>
            <div class="error-notification" v-if="showErrors.name && errors.name">
              <Icon icon="mdi:alert-circle" class="error-icon" />
              <span class="error-text">{{ errors.name }}</span>
            </div>
          </div>
          
          <div class="form-group">
            <label for="phone">Телефон *</label>
            <div class="input-wrapper">
              <Icon icon="mdi:phone-outline" class="input-icon" />
              <input 
                type="tel" 
                id="phone" 
                v-model="form.phone" 
                @input="formatRussianPhone"
                @focus="hideError('phone')"
                @blur="validatePhone"
                :class="{ 'invalid': errors.phone }"
                placeholder="+7 (999) 123-45-67"
                class="form-input"
              >
            </div>
            <div class="error-notification" v-if="showErrors.phone && errors.phone">
              <Icon icon="mdi:alert-circle" class="error-icon" />
              <span class="error-text">{{ errors.phone }}</span>
            </div>
          </div>
          
          <div class="form-group">
            <label for="tour">Выберите прогулку *</label>
            <div class="input-wrapper">
              <Icon icon="mdi:sail-boat" class="input-icon" />
              <select 
                id="tour" 
                v-model="form.selectedTour"
                class="form-select"
                required
              >
                <option 
                  v-for="tour in tours" 
                  :key="tour.id" 
                  :value="tour.id"
                >
                  {{ tour.title }} ({{ tour.basePrice }} ₽)
                </option>
              </select>
            </div>
          </div>
          
          <div class="form-row">
            <div class="form-group">
              <label for="people">Количество человек *</label>
              <div class="input-wrapper">
                <Icon icon="mdi:account-group-outline" class="input-icon" />
                <input
                  type="number"
                  id="people"
                  v-model.number="form.peopleCount"
                  @focus="hideError('peopleCount')"
                  @blur="validatePeopleCount"
                  min="1"
                  max="7"
                  required
                  :class="{ 'invalid': errors.peopleCount }"
                  class="form-input"
                >
              </div>
              <div class="error-notification" v-if="showErrors.peopleCount && errors.peopleCount">
                <Icon icon="mdi:alert-circle" class="error-icon" />
                <span class="error-text">{{ errors.peopleCount }}</span>
              </div>
            </div>
            
            <div class="form-group">
              <label for="date">Дата *</label>
              <div class="input-wrapper">
                <Icon icon="mdi:calendar-outline" class="input-icon" />
                <input
                  type="date"
                  id="date"
                  v-model="form.date"
                  @focus="hideError('date')"
                  @blur="validateDate"
                  :min="formatDateForInput(new Date())"
                  :max="formatDateForInput(new Date(new Date().setDate(new Date().getDate() + 30)))"
                  :class="{ 'invalid': errors.date }"
                  class="form-input"
                >
              </div>
              <div class="error-notification" v-if="showErrors.date && errors.date">
                <Icon icon="mdi:alert-circle" class="error-icon" />
                <span class="error-text">{{ errors.date }}</span>
              </div>
            </div>
          </div>
          
          <div class="form-group">
            <label for="time">Время начала *</label>
            <div class="input-wrapper">
              <Icon icon="mdi:clock-outline" class="input-icon" />
              <select
                id="time"
                v-model="form.time"
                @focus="hideError('time')"
                @blur="validateTime"
                :class="{ 'invalid': errors.time }"
                class="form-select"
                :disabled="!form.date"
              >
                <option value="" disabled>Выберите время</option>
                <option 
                  v-for="slot in availableTimeSlots" 
                  :key="slot" 
                  :value="slot"
                >
                  {{ slot }}
                </option>
              </select>
            </div>
            <div class="error-notification" v-if="showErrors.time && errors.time">
              <Icon icon="mdi:alert-circle" class="error-icon" />
              <span class="error-text">{{ errors.time }}</span>
            </div>
          </div>
          
          <div class="price-calculation">
            <div class="price-content">
              <h3>Итоговая стоимость:</h3>
              <div class="price-amount">{{ totalPrice }} ₽</div>
              <div class="price-note" v-if="form.peopleCount > 2">
                (6000 ₽ за 1-2 человек + {{ (form.peopleCount - 2) * 1000 }} ₽ за доп. гостей)
              </div>
            </div>
            <Icon icon="mdi:cash-multiple" class="price-icon" />
          </div>
          
          <div class="form-group">
            <label for="message">Дополнительные пожелания</label>
            <div class="input-wrapper">
              <Icon icon="mdi:message-text-outline" class="input-icon textarea-icon" />
              <textarea 
                id="message" 
                v-model="form.message" 
                rows="3"
                placeholder="Особые пожелания, время прогулки и т.д."
                class="form-textarea"
              ></textarea>
            </div>
          </div>
          
          <div class="form-actions">
            <button 
              type="submit" 
              class="submit-button" 
              :disabled="isSubmitting || !isFormValid"
            >
              <span v-if="!isSubmitting">Забронировать за {{ totalPrice }} ₽</span>
              <span v-else class="submit-loading">
                <span class="loading-dots">
                  <span>.</span><span>.</span><span>.</span>
                </span>
              </span>
            </button>

            <button 
              type="button" 
              class="telegram-button" 
              @click="contactViaTelegram"
            >
              <Icon icon="mdi:telegram" class="telegram-icon" />
              <span>Связаться в Telegram</span>
            </button>
          </div>
          
          <transition name="fade">
            <div v-if="submitSuccess" class="success-message">
              <div class="success-content">
                <Icon icon="mdi:check-circle-outline" class="success-icon" />
                <div>
                  <h3>Спасибо за бронирование!</h3>
                  <p>Мы свяжемся с вами в течение 15 минут для подтверждения.</p>
                  <p>Номер заявки: #{{ Math.floor(Math.random() * 10000) }}</p>
                </div>
              </div>
            </div>
          </transition>
        </form>
        
        <div class="booking-summary">
          <h3 class="summary-title">Ваша бронь</h3>
          <div class="summary-items">
            <div class="summary-item">
              <Icon icon="mdi:sail-boat" class="summary-icon" />
              <div>
                <span class="summary-label">Прогулка:</span>
                <strong class="summary-value">{{ tours.find(t => t.id === form.selectedTour)?.title }}</strong>
              </div>
            </div>
            <div class="summary-item">
              <Icon icon="mdi:account-group-outline" class="summary-icon" />
              <div>
                <span class="summary-label">Количество человек:</span>
                <strong class="summary-value">{{ form.peopleCount }}</strong>
              </div>
            </div>
            <div class="summary-item" v-if="form.date">
              <Icon icon="mdi:calendar-outline" class="summary-icon" />
              <div>
                <span class="summary-label">Дата:</span>
                <strong class="summary-value">{{ formatDate(new Date(form.date)) }}</strong>
              </div>
            </div>
            <div class="summary-item" v-if="form.time">
              <Icon icon="mdi:clock-outline" class="summary-icon" />
              <div>
                <span class="summary-label">Время:</span>
                <strong class="summary-value">{{ form.time }}</strong>
              </div>
            </div>
            <div class="summary-item price">
              <Icon icon="mdi:cash-multiple" class="summary-icon" />
              <div>
                <span class="summary-label">Стоимость:</span>
                <strong class="summary-value">{{ totalPrice }} ₽</strong>
              </div>
            </div>
          </div>
          
          <div class="contact-info">
            <h4 class="contact-title">Контактная информация</h4>
            <div class="contact-items">
              <div class="contact-item">
                <Icon icon="mdi:map-marker-outline" />
                <span>г. Владивосток, ул. Токаревский маяк, 1</span>
              </div>
              <div class="contact-item">
                <Icon icon="mdi:phone-outline" />
                <span>+7 (908) 992-06-49</span>
              </div>
              <div class="contact-item">
                <Icon icon="mdi:email-outline" />
                <span>zhigalinleonid@gmail.com</span>
              </div>
              <div class="contact-item">
                <Icon icon="mdi:clock-outline" />
                <span>ежедневно с 8:00 до 20:00</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&family=Montserrat:wght@300;400;600&display=swap');

.contact-section {
  padding: 6rem 0;
  background-color: #f8fafc;
  position: relative;
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease-out;
}

.contact-section.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-header {
  text-align: center;
  margin-bottom: 3rem;
}

.section-title {
  font-family: 'Montserrat', sans-serif;
  font-size: clamp(1.8rem, 4vw, 2.5rem);
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 1.5rem;
}

.section-divider {
  width: 80px;
  height: 4px;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  margin: 0 auto;
  border-radius: 2px;
}

.contact-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2.5rem;
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  position: relative;
}

.form-group label {
  font-family: 'Manrope', sans-serif;
  font-weight: 600;
  color: #1e293b;
  font-size: 0.95rem;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.input-icon {
  position: absolute;
  left: 1rem;
  color: #64748b;
  font-size: 1.2rem;
  z-index: 2;
}

.textarea-icon {
  align-self: flex-start;
  margin-top: 1rem;
}

.form-input,
.form-select,
.form-textarea {
  padding: 0.75rem 1rem 0.75rem 3rem;
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  font-family: 'Manrope', sans-serif;
  font-size: 1rem;
  transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
  background-color: white;
  width: 100%;
  box-sizing: border-box;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
}

.form-input:focus,
.form-select:focus,
.form-textarea:focus {
  outline: none;
  border-color: #4fc1e9;
  box-shadow: 0 0 0 3px rgba(79, 193, 233, 0.2);
}

.form-textarea {
  min-height: 100px;
  resize: vertical;
  padding-top: 1rem;
}

.invalid {
  border-color: #ef4444;
  background-color: #fef2f2;
}

.invalid + .input-icon {
  color: #ef4444;
}

.error-notification {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #ef4444;
  font-size: 0.8rem;
  margin-top: 0.3rem;
  font-family: 'Manrope', sans-serif;
}

.error-icon {
  font-size: 1rem;
  flex-shrink: 0;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

.price-calculation {
  background: linear-gradient(135deg, #f8fafc, #e2e8f0);
  padding: 1.5rem;
  border-radius: 10px;
  margin: 1rem 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #e2e8f0;
}

.price-content {
  flex: 1;
}

.price-calculation h3 {
  font-family: 'Manrope', sans-serif;
  font-size: 1.1rem;
  font-weight: 600;
  color: #1e293b;
  margin: 0 0 0.5rem 0;
}

.price-amount {
  font-family: 'Manrope', sans-serif;
  font-size: 1.8rem;
  font-weight: 800;
  color: #4fc1e9;
  margin: 0.5rem 0;
}

.price-note {
  font-family: 'Manrope', sans-serif;
  font-size: 0.85rem;
  color: #64748b;
}

.price-icon {
  font-size: 2.5rem;
  color: #cbd5e1;
  margin-left: 1rem;
}

.form-actions {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 1rem;
}

.submit-button {
  padding: 1rem;
  background: linear-gradient(135deg, #4fc1e9, #3da7cc);
  color: white;
  border: none;
  border-radius: 10px;
  font-family: 'Manrope', sans-serif;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  box-shadow: 0 4px 6px rgba(63, 167, 204, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
}

.submit-button:hover:not(:disabled) {
  transform: translateY(-3px);
  box-shadow: 0 8px 15px rgba(63, 167, 204, 0.4);
}

.submit-button:disabled {
  opacity: 0.7;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.submit-loading {
  display: inline-flex;
}

.loading-dots span {
  animation: loadingDot 1.4s infinite both;
  display: inline-block;
}

.loading-dots span:nth-child(2) {
  animation-delay: 0.2s;
}

.loading-dots span:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes loadingDot {
  0%, 80%, 100% { opacity: 0; }
  40% { opacity: 1; }
}

.telegram-button {
  padding: 1rem;
  background: linear-gradient(135deg, #0088cc, #0077b5);
  color: white;
  border: none;
  border-radius: 10px;
  font-family: 'Manrope', sans-serif;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  box-shadow: 0 4px 6px rgba(0, 136, 204, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.telegram-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 15px rgba(0, 136, 204, 0.4);
}

.telegram-icon {
  font-size: 1.3rem;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.success-message {
  padding: 1.5rem;
  background-color: #d4edda;
  color: #155724;
  border-radius: 10px;
  margin-top: 1.5rem;
}

.success-content {
  display: flex;
  gap: 1rem;
}

.success-icon {
  font-size: 2rem;
  color: #155724;
  flex-shrink: 0;
}

.success-message h3 {
  font-family: 'Manrope', sans-serif;
  font-size: 1.2rem;
  font-weight: 700;
  margin: 0 0 0.5rem 0;
  color: #155724;
}

.success-message p {
  font-family: 'Manrope', sans-serif;
  font-size: 0.9rem;
  margin: 0.3rem 0;
  color: #155724;
}

.booking-summary {
  background: white;
  padding: 2rem;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  align-self: start;
  position: sticky;
  top: 1rem;
}

.summary-title {
  font-family: 'Montserrat', sans-serif;
  font-size: 1.5rem;
  font-weight: 700;
  color: #1e293b;
  margin: 0 0 1.5rem 0;
  padding-bottom: 1rem;
  border-bottom: 1px solid #e2e8f0;
}

.summary-items {
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
}

.summary-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding-bottom: 1.2rem;
  border-bottom: 1px solid #f1f5f9;
}

.summary-item.price {
  padding-top: 1rem;
  border-top: 2px solid #4fc1e9;
  border-bottom: none;
  margin-top: 0.5rem;
}

.summary-icon {
  font-size: 1.5rem;
  color: #64748b;
  margin-top: 0.2rem;
  flex-shrink: 0;
}

.summary-label {
  font-family: 'Manrope', sans-serif;
  font-size: 0.9rem;
  color: #64748b;
  display: block;
  margin-bottom: 0.3rem;
}

.summary-value {
  font-family: 'Manrope', sans-serif;
  font-size: 1rem;
  font-weight: 600;
  color: #1e293b;
}

.summary-item.price .summary-value {
  font-size: 1.3rem;
  color: #4fc1e9;
}

.contact-info {
  margin-top: 2.5rem;
  padding-top: 2rem;
  border-top: 1px solid #e2e8f0;
}

.contact-title {
  font-family: 'Montserrat', sans-serif;
  font-size: 1.2rem;
  font-weight: 600;
  color: #1e293b;
  margin: 0 0 1.5rem 0;
}

.contact-items {
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-family: 'Manrope', sans-serif;
  color: #475569;
}

.contact-item svg {
  font-size: 1.3rem;
  color: #64748b;
  flex-shrink: 0;
}

@media (max-width: 1024px) {
  .contact-container {
    grid-template-columns: 1fr;
    max-width: 700px;
    margin: 0 auto;
  }
  
  .booking-summary {
    position: static;
    order: -1;
  }
  
  .form-row {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .contact-section {
    padding: 4rem 0;
  }
  
  .price-amount {
    font-size: 1.5rem;
  }
  
  .price-icon {
    font-size: 2rem;
  }
}

@media (max-width: 480px) {
  .section-title {
    font-size: 1.8rem;
  }
  
  .contact-form {
    gap: 1.2rem;
  }
  
  .price-calculation {
    flex-direction: column;
    align-items: flex-start;
    gap: 1rem;
  }
  
  .price-icon {
    display: none;
  }
}
</style>