<script setup>
import { ref } from 'vue'

const isMenuOpen = ref(false)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}

const scrollToSection = (id) => {
  document.querySelector(id)?.scrollIntoView({
    behavior: 'smooth'
  })
  isMenuOpen.value = false // Закрываем меню после клика на мобильных устройствах
}
</script>

<template>
  <header class="header">
    <!-- Волновой фон -->
    <div class="wave-bg">
      <svg viewBox="0 0 1200 120" preserveAspectRatio="none">
        <path d="M0,0V46.29c47.79,22.2,103.59,32.17,158,28,70.36-5.37,136.33-33.31,206.8-37.5C438.64,32.43,512.34,53.67,583,72.05c69.27,18,138.3,24.88,209.4,13.08,36.15-6,69.85-17.84,104.45-29.34C989.49,25,1113-14.29,1200,52.47V0Z" opacity=".25" fill="currentColor"></path>
        <path d="M0,0V15.81C13,36.92,27.64,56.86,47.69,72.05,99.41,111.27,165,111,224.58,91.58c31.15-10.15,60.09-26.07,89.67-39.8,40.92-19,84.73-46,130.83-49.67,36.26-2.85,70.9,9.42,98.6,31.56,31.77,25.39,62.32,62,103.63,73,40.44,10.79,81.35-6.69,119.13-24.28s75.16-39,116.92-43.05c59.73-5.85,113.28,22.88,168.9,38.84,30.2,8.66,59,6.17,87.09-7.5,22.43-10.89,48-26.93,60.65-49.24V0Z" opacity=".5" fill="currentColor"></path>
        <path d="M0,0V5.63C149.93,59,314.09,71.32,475.83,42.57c43-7.64,84.23-20.12,127.61-26.46,59-8.63,112.48,12.24,165.56,35.4C827.93,77.22,886,95.24,951.2,90c86.53-7,172.46-45.71,248.8-84.81V0Z" fill="currentColor"></path>
      </svg>
    </div>
    
    <div class="container">
      <div class="logo">FastMarine</div>
      <nav class="nav" :class="{ active: isMenuOpen }">
        <ul>
          <li><button @click="scrollToSection('#about')" class="nav-button">О нас</button></li>
          <li><button @click="scrollToSection('#tours')" class="nav-button">Прогулки</button></li>
          <li><button @click="scrollToSection('#gallery')" class="nav-button">Галерея</button></li>
          <li><button @click="scrollToSection('#fleet')" class="nav-button">Флот</button></li>
          <li><button @click="scrollToSection('#contact')" class="nav-button">Контакты</button></li>
        </ul>
      </nav>
      <button class="menu-toggle" @click="toggleMenu">
        <span class="menu-icon">☰</span>
      </button>
    </div>
  </header>
</template>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: rgba(2, 86, 107, 0.9);
  padding: 1.5rem 0;
  z-index: 1000;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.wave-bg {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
  line-height: 0;
  transform: rotate(180deg);
}

.wave-bg svg {
  position: relative;
  display: block;
  width: calc(100% + 1.3px);
  height: 60px;
  color: rgba(1, 68, 85, 0.7);
}

.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  position: relative;
}

.logo {
  color: white;
  font-size: 2rem;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 1px;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.3);
}

.nav ul {
  display: flex;
  list-style: none;
  gap: 2.5rem;
  margin: 0;
  padding: 0;
}

.nav-button {
  color: white;
  background: none;
  border: none;
  cursor: pointer;
  text-decoration: none;
  font-size: 1.2rem;
  font-weight: 500;
  letter-spacing: 0.5px;
  padding: 0.5rem 0;
  position: relative;
  transition: all 0.3s ease;
  font-family: inherit;
}

.nav-button:hover {
  color: #4fc1e9;
}

.nav-button::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background-color: #4fc1e9;
  transition: width 0.3s ease;
}

.nav-button:hover::after {
  width: 100%;
}

.menu-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
}

.menu-icon {
  color: white;
  font-size: 2rem;
  line-height: 1;
}

@media (max-width: 768px) {
  .header {
    padding: 1rem 0;
  }
  
  .logo {
    font-size: 1.8rem;
    visibility: visible;
  }
  
  .nav ul {
    position: fixed;
    top: 80px;
    left: -100%;
    flex-direction: column;
    background-color: rgba(2, 86, 107, 0.95);
    width: 100%;
    padding: 2rem 0;
    gap: 1.5rem;
    align-items: center;
    transition: left 0.3s ease;
    box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  }
  
  .nav.active ul {
    left: 0;
  }
  
  .nav-button {
    font-size: 1.3rem;
    padding: 0.5rem 1rem;
  }
  
  .menu-toggle {
    display: block;
    z-index: 1001;
  }
  
  .wave-bg svg {
    height: 40px;
  }
}

@media (max-width: 480px) {
  .logo {
    font-size: 1.5rem;
  }
  
  .menu-icon {
    font-size: 1.8rem;
  }
  
  .nav ul {
    top: 70px;
  }
}
</style>