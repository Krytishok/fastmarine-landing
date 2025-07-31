<script setup>
import { onMounted, ref } from 'vue'

const currentYear = new Date().getFullYear()
const footerVisible = ref(false)

const socialLinks = [
  { icon: '📱', name: 'WhatsApp', url: '#' },
  { icon: '📷', name: 'Instagram', url: '#' },
  { icon: '📘', name: 'Facebook', url: '#' }
]

onMounted(() => {
  footerVisible.value = true
})
</script>

<template>
  <footer class="footer" :class="{ 'visible': footerVisible }">
    <div class="container">
      <div class="footer-content">
        <!-- Блок 1 - О компании -->
        <div class="footer-section">
          <h3 class="footer-title">FASTMARINE</h3>
          <div class="divider"></div>
          <p>Морские прогулки и экскурсии в г. Владивосток</p>
        </div>
        
        <!-- Блок 2 - Навигация -->
        <div class="footer-section">
          <h3 class="footer-title">Навигация</h3>
          <div class="divider"></div>
          <ul class="footer-links">
            <li><a href="#about">О нас</a></li>
            <li><a href="#tours">Прогулки</a></li>
            <li><a href="#contact">Контакты</a></li>
          </ul>
        </div>
        
        <!-- Блок 3 - Контакты -->
        <div class="footer-section">
          <h3 class="footer-title">Контакты</h3>
          <div class="divider"></div>
          <div class="contact-item">
            <span>📍</span>
            <p>г. Владивосток, ул. Токаревский маяк, 1</p>
          </div>
          <div class="contact-item">
            <span>📞</span>
            <a href="tel:+79089920649">+7 (908) 992-06-49</a>
          </div>
          <div class="contact-item">
            <span>✉️</span>
            <a href="mailto:zhigalinleonid@gmail.com">zhigalinleonid@gmail.com</a>
          </div>
        </div>
        
        <!-- Блок 4 - Соцсети -->
        <div class="footer-section">
          <h3 class="footer-title">Соцсети</h3>
          <div class="divider"></div>
          <div class="social-links">
            <a 
              v-for="(link, index) in socialLinks" 
              :key="index" 
              :href="link.url" 
              :title="link.name"
              class="social-link"
            >
              {{ link.icon }} {{ link.name }}
            </a>
          </div>
        </div>
      </div>
      
      <div class="footer-bottom">
        <p>&copy; {{ currentYear }} OOO Паллада. Все права защищены.</p>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: #0d1a26;
  color: white;
  padding: 3rem 0 1rem;
  font-family: 'Arial', sans-serif;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.footer.visible {
  opacity: 1;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.footer-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 2rem;
  margin-bottom: 2rem;
}

.footer-section {
  margin-bottom: 1.5rem;
}

.footer-title {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 1rem;
  color: #4fc1e9;
}

.divider {
  height: 2px;
  width: 50px;
  background: #4fc1e9;
  margin: 0.5rem 0 1rem;
}

.footer-section p,
.footer-section a {
  color: #ddd;
  line-height: 1.6;
  font-size: 0.95rem;
}

.footer-section a {
  text-decoration: none;
  transition: color 0.3s;
}

.footer-section a:hover {
  color: #4fc1e9;
}

.footer-links {
  list-style: none;
  padding: 0;
}

.footer-links li {
  margin-bottom: 0.5rem;
}

.contact-item {
  display: flex;
  gap: 10px;
  margin-bottom: 0.8rem;
  align-items: center;
}

.contact-item span {
  color: #4fc1e9;
}

.social-links {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}

.social-link {
  color: #ddd;
  transition: color 0.3s;
}

.social-link:hover {
  color: #4fc1e9;
}

.footer-bottom {
  text-align: center;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  color: #aaa;
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .footer-content {
    grid-template-columns: 1fr;
  }
  
  .footer-section {
    margin-bottom: 2rem;
  }
}
</style>