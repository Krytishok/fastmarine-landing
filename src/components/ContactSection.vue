<script setup>
import { onMounted, ref } from 'vue'

const animated = ref(false)
const phoneNumber = '+79089920649'

// Формируем корректные ссылки для мессенджеров
const telegramLink = `https://t.me/Freeman19811`
const whatsappLink = `https://wa.me/${phoneNumber.replace('+', '')}`

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    if (entries[0].isIntersecting) {
      animated.value = true
      observer.disconnect()
    }
  }, { threshold: 0.1 })

  const section = document.querySelector('.contact-section')
  if (section) observer.observe(section)
})

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}
</script>

<template>
  <section id="contact" class="contact-section">
    <div class="container">
      <!-- Заголовок секции -->
      <div class="section-header" :class="{ 'animate-in': animated }">
        <h2 class="section-title">Забронируйте прогулку</h2>
        <div class="section-divider"></div>
        <p class="section-subtitle">Свяжитесь с нами, чтобы организовать вашу идеальную прогулку.</p>
      </div>
      
      <!-- Основное содержимое -->
      <div class="contact-content" :class="{ 'animate-in': animated }">
        <div class="contact-card">
          <!-- Блок с контактной информацией -->
          <div class="contact-info">
            <div class="info-item">
              <Icon icon="mdi:clock-outline" class="info-icon" />
              <div>
                <h3>Часы работы</h3>
                <p>Ежедневно с 8:00 до 20:00</p>
              </div>
            </div>
            
            <div class="info-item">
              <Icon icon="mdi:map-marker" class="info-icon" />
              <div>
                <h3>Место отправления</h3>
                <p>г. Владивосток, Токаревский маяк, 1 (Яхт-клуб)</p>
              </div>
            </div>
            
            <div class="info-item">
              <Icon icon="mdi:phone" class="info-icon" />
              <div>
                <h3>Телефон</h3>
                <a :href="`tel:${phoneNumber}`" class="phone-link">{{ phoneNumber }}</a>
              </div>
            </div>
          </div>
          
          <!-- Блок с кнопками -->
          <div class="contact-buttons">
            <h3 class="buttons-title">Как с нами связаться?</h3>
            
            <!-- Кнопка WhatsApp -->
            <a 
              :href="whatsappLink" 
              target="_blank" 
              rel="noopener noreferrer"
              class="messenger-button whatsapp"
              :style="{ '--delay': '0.2s' }"
            >
              <Icon icon="mdi:whatsapp" class="button-icon" />
              <span>WhatsApp</span>
            </a>
            
            <!-- Кнопка Telegram -->
            <a 
              :href="telegramLink" 
              target="_blank" 
              rel="noopener noreferrer"
              class="messenger-button telegram"
              :style="{ '--delay': '0.3s' }"
            >
              <Icon icon="mdi:telegram" class="button-icon" />
              <span>Telegram</span>
            </a>
            
            <!-- Кнопка звонка -->
            <a 
              :href="`tel:${phoneNumber}`" 
              class="messenger-button phone"
              :style="{ '--delay': '0.4s' }"
            >
              <Icon icon="mdi:phone" class="button-icon" />
              <span>Позвонить</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.contact-section {
  padding: 6rem 0;
  background: linear-gradient(135deg, #f5fcff 0%, #e0f4ff 100%);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-header {
  text-align: center;
  margin-bottom: 3rem;
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease;
}

.section-header.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.section-title {
  font-size: 2rem;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 1rem;
}

.section-divider {
  width: 80px;
  height: 4px;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  margin: 0 auto 1.5rem;
  border-radius: 2px;
  transform: scaleX(0);
  transform-origin: center;
  transition: transform 0.8s ease 0.3s;
}

.section-header.animate-in .section-divider {
  transform: scaleX(1);
}

.section-subtitle {
  font-size: 1.2rem;
  color: #4a5568;
  max-width: 600px;
  margin: 0 auto;
}

.contact-content {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2.5rem;
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease 0.2s;
}

.contact-content.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.contact-card {
  background: white;
  border-radius: 16px;
  padding: 2.5rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.info-item {
  display: flex;
  gap: 1.2rem;
  align-items: flex-start;
}

.info-icon {
  font-size: 1.8rem;
  color: #4fc1e9;
  flex-shrink: 0;
  margin-top: 3px;
}

.info-item h3 {
  font-size: 1.2rem;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 0.5rem;
}

.info-item p, .phone-link {
  color: #4a5568;
  line-height: 1.6;
}

.phone-link {
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s;
}

.phone-link:hover {
  color: #4fc1e9;
}

.contact-buttons {
  display: flex;
  flex-direction: column;
}

.buttons-title {
  font-size: 1.2rem;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 1.5rem;
}

.messenger-button {
  position: relative;
  padding: 1rem 1.5rem;
  border-radius: 12px;
  font-weight: 600;
  text-decoration: none;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-bottom: 1rem;
  transition: all 0.3s ease;
  color: white;
  opacity: 0;
  transform: translateY(20px);
}

.contact-content.animate-in .messenger-button {
  animation: buttonFadeIn 0.8s cubic-bezier(0.22, 1, 0.36, 1) forwards;
  animation-delay: var(--delay);
}

.whatsapp {
  background: #25D366;
}

.telegram {
  background: #0088cc;
}

.phone {
  background: #4fc1e9;
}

.button-icon {
  font-size: 1.5rem;
}

.messenger-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.guarantee-card {
  background: rgba(79, 193, 233, 0.1);
  border-radius: 16px;
  padding: 2rem;
  border: 1px solid rgba(79, 193, 233, 0.2);
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease 0.4s;
}

.guarantee-card.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.guarantee-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: #1e293b;
  text-align: center;
  margin-bottom: 1.5rem;
}

.divider {
  width: 60px;
  height: 3px;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  margin: 0 auto 2rem;
  border-radius: 2px;
}

.guarantees {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.5rem;
}

.guarantee-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.guarantee-icon {
  font-size: 2.5rem;
  color: #4fc1e9;
  margin-bottom: 1rem;
}

.guarantee-item p {
  color: #4a5568;
  line-height: 1.5;
}

.scroll-top-button {
  position: fixed;
  bottom: 30px;
  right: 30px;
  background: #4fc1e9;
  color: white;
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 5px 20px rgba(63, 167, 204, 0.3);
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.4s ease;
  z-index: 100;
}

.scroll-top-button.show {
  opacity: 1;
  transform: translateY(0);
}

.scroll-top-button:hover {
  background: #3da7cc;
  transform: translateY(-5px);
}

@keyframes buttonFadeIn {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .contact-section {
    padding: 4rem 0;
  }
  
  .contact-card {
    grid-template-columns: 1fr;
    padding: 1.5rem;
  }
  
  .guarantees {
    grid-template-columns: 1fr 1fr;
  }
  
  .messenger-button {
    padding: 1rem;
  }
}

@media (max-width: 480px) {
  .section-title {
    font-size: 1.8rem;
  }
  
  .section-subtitle {
    font-size: 1rem;
  }
  
  .guarantees {
    grid-template-columns: 1fr;
  }
  
  .scroll-top-button {
    bottom: 20px;
    right: 20px;
    width: 45px;
    height: 45px;
  }
}
</style>