<script setup>
import { onMounted } from 'vue'

const scrollToTours = () => {
  document.querySelector('#tours')?.scrollIntoView({
    behavior: 'smooth'
  })
}

onMounted(() => {
  // Параллакс эффект для фона
  const hero = document.querySelector('.hero')
  if (hero) {
    window.addEventListener('scroll', () => {
      const scrollValue = window.scrollY
      hero.style.backgroundPositionY = `${scrollValue * 0.5}px`
    })
  }
})
</script>

<template>
  <section class="hero">
    <div class="hero-content">
      <div class="title-wrapper">
        <h1 class="title-main">Морские прогулки</h1>
        <div class="title-line"></div>
      </div>
      <p class="subtitle">Откройте для себя красоту островов Приморского Края</p>
      <button class="cta-button" @click="scrollToTours">
        <span class="button-text">Выбрать прогулку</span>
        <span class="button-icon">→</span>
      </button>
    </div>
    
    <div class="scroll-hint">
      <div class="scroll-hint-line"></div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&family=Montserrat:wght@300;400;600&display=swap');

.hero {
  position: relative;
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow: hidden;
  font-family: 'Manrope', sans-serif;
  background: linear-gradient(135deg, rgba(0, 24, 48, 0.7) 0%, rgba(0, 48, 96, 0.5) 100%), 
              url('../assets/images/hero-bg.webp') no-repeat center center/cover;
  background-attachment: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
}

.hero-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: white;
  padding: 0 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.title-wrapper {
  margin-bottom: 2rem;
  overflow: hidden;
}

.title-main {
  font-family: 'Montserrat', sans-serif;
  font-size: clamp(3rem, 6vw, 5rem);
  font-weight: 800;
  margin: 0;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
  line-height: 1.1;
  letter-spacing: 1.5px;
  transform: translateY(100%);
  opacity: 0;
  animation: titleReveal 1.2s cubic-bezier(0.22, 1, 0.36, 1) forwards;
}

.title-line {
  height: 4px;
  width: 0;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  margin: 1rem auto 0;
  animation: lineReveal 1s ease-out forwards;
  animation-delay: 0.8s;
  border-radius: 2px;
}

.subtitle {
  font-size: clamp(1.2rem, 2.5vw, 1.8rem);
  font-weight: 300;
  margin-bottom: 3rem;
  max-width: 700px;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeUp 1s ease-out forwards;
  animation-delay: 1s;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
  line-height: 1.5;
}

.cta-button {
  position: relative;
  background: transparent;
  color: white;
  border: 2px solid rgba(255, 255, 255, 0.3);
  padding: 1.2rem 2.5rem;
  font-size: 1.1rem;
  font-weight: 600;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  overflow: hidden;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeUp 1s ease-out forwards;
  animation-delay: 1.2s;
  display: flex;
  align-items: center;
  gap: 10px;
}

.cta-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #4fc1e9, #3da7cc);
  z-index: -1;
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.6s cubic-bezier(0.23, 1, 0.32, 1);
}

.cta-button:hover {
  border-color: transparent;
  transform: translateY(-5px);
  box-shadow: 0 15px 30px rgba(31, 140, 201, 0.3);
}

.cta-button:hover::before {
  transform: scaleX(1);
  transform-origin: left;
}

.cta-button:hover .button-icon {
  transform: translateX(5px);
}

.button-icon {
  transition: transform 0.3s ease;
  font-size: 1.2em;
}

.scroll-hint {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  opacity: 0;
  animation: fadeIn 1s ease-out forwards;
  animation-delay: 1.8s;
}

.scroll-hint-line {
  width: 1px;
  height: 40px;
  background: linear-gradient(to bottom, rgba(255,255,255,1), rgba(255,255,255,0));
  animation: scrollHint 2s infinite;
}

@keyframes titleReveal {
  from {
    transform: translateY(100%);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes lineReveal {
  from {
    width: 0;
  }
  to {
    width: 200px;
  }
}

@keyframes fadeUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeIn {
  to {
    opacity: 1;
  }
}

@keyframes scrollHint {
  0% {
    transform: translateY(-20px);
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform: translateY(20px);
    opacity: 0;
  }
}

@media (max-width: 768px) {
  .title-main {
    font-size: clamp(2.5rem, 8vw, 4rem);
  }
  
  .subtitle {
    font-size: clamp(1.1rem, 4vw, 1.5rem);
  }
  
  .cta-button {
    padding: 1rem 2rem;
  }
  
  .title-line {
    height: 3px;
  }

  .hero{
    background: linear-gradient(135deg, rgba(0, 24, 48, 0.7) 0%, rgba(0, 48, 96, 0.5) 100%), 
              url('../assets/images/mayak-mobile.png') no-repeat center center/cover;
  }
}
</style>