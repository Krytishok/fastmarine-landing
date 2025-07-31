<script setup>
import { onMounted, ref } from 'vue'

const features = [
  {
    icon: '🚤',
    title: 'Быстроходные катера',
    description: 'Успеете посмотреть все красоты и прокатиться с ветерком'
  },
  {
    icon: '👱',
    title: 'Индивидуальные прогулки',
    description: 'Не нужно ждать, пока наберется группа, можно отправиться сразу'
  },
  {
    icon: '🏊',
    title: 'Купание с катера',
    description: 'В рамках прогулки есть возможность искупаться в бухте Новик рядом с Русским островом'
  }
]

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

  const section = document.querySelector('.about')
  if (section) observer.observe(section)
})
</script>

<template>
  <section id="about" class="about">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">Почему выбирают нас</h2>
        <div class="section-divider"></div>
      </div>
      
      <div class="features-grid">
        <div 
          class="feature-card" 
          v-for="(feature, index) in features" 
          :key="index"
          :class="{ 'animate-in': animated }"
          :style="{ '--delay': index * 0.1 + 's' }"
        >
          <div class="feature-icon-wrapper">
            <div class="feature-icon">{{ feature.icon }}</div>
            <div class="icon-pulse"></div>
          </div>
          <h3 class="feature-title">{{ feature.title }}</h3>
          <p class="feature-description">{{ feature.description }}</p>
          <div class="feature-wave"></div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&family=Montserrat:wght@300;400;600&display=swap');

.about {
  padding: 6rem 0;
  background-color: #f8fafc;
  position: relative;
  overflow: hidden;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  position: relative;
  z-index: 1;
}

.section-header {
  margin-bottom: 4rem;
  text-align: center;
}

.section-title {
  font-family: 'Montserrat', sans-serif;
  font-size: clamp(1.8rem, 4vw, 2.5rem);
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 1.5rem;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeUp 0.8s ease-out forwards;
}

.section-divider {
  width: 80px;
  height: 4px;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  margin: 0 auto;
  border-radius: 2px;
  opacity: 0;
  transform: scaleX(0);
  transform-origin: center;
  animation: scaleIn 0.8s ease-out forwards;
  animation-delay: 0.3s;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2.5rem;
}

.feature-card {
  position: relative;
  text-align: center;
  padding: 2.5rem 2rem;
  border-radius: 16px;
  background: white;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  overflow: hidden;
  opacity: 0;
  transform: translateY(30px);
}

.feature-card.animate-in {
  animation: cardFadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) forwards;
  animation-delay: var(--delay);
}

.feature-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 15px 40px rgba(31, 140, 201, 0.15);
}

.feature-icon-wrapper {
  position: relative;
  width: 80px;
  height: 80px;
  margin: 0 auto 1.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.feature-icon {
  font-size: 2.5rem;
  z-index: 2;
  transition: transform 0.3s ease;
}

.feature-card:hover .feature-icon {
  transform: scale(1.1);
}

.icon-pulse {
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(79, 193, 233, 0.1);
  border-radius: 50%;
  z-index: 1;
  opacity: 0;
  transition: all 0.6s ease;
}

.feature-card:hover .icon-pulse {
  opacity: 1;
  transform: scale(1.4);
}

.feature-title {
  font-family: 'Manrope', sans-serif;
  font-size: 1.3rem;
  font-weight: 700;
  margin-bottom: 1rem;
  color: #1e293b;
  transition: color 0.3s ease;
}

.feature-card:hover .feature-title {
  color: #3da7cc;
}

.feature-description {
  font-family: 'Manrope', sans-serif;
  font-size: 1rem;
  line-height: 1.6;
  color: #64748b;
  margin-bottom: 0;
  transition: color 0.3s ease;
}

.feature-card:hover .feature-description {
  color: #475569;
}

.feature-wave {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.6s cubic-bezier(0.23, 1, 0.32, 1);
}

.feature-card:hover .feature-wave {
  transform: scaleX(1);
}

@keyframes fadeUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes scaleIn {
  to {
    opacity: 1;
    transform: scaleX(1);
  }
}

@keyframes cardFadeUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .about {
    padding: 4rem 0;
  }
  
  .features-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
  
  .feature-card {
    padding: 2rem 1.5rem;
  }
}
</style>