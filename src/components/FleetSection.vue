<script setup>
import { ref, onMounted } from 'vue'

const fleet = [
  {
    id: 1,
    name: "Bayliner Capri",
    image: new URL('../assets/images/bayliner.jpg', import.meta.url).href,
    specs: [
      { label: "Длина", value: "5.2 м", icon: "mdi:ruler" },
      { label: "Вместимость", value: "6 человек", icon: "mdi:account-group" },
      { label: "Мощность", value: "150 л.с.", icon: "mdi:engine" }
    ],
    features: [
      "Удобные мягкие сиденья",
      "Солнечный тент",
      "Стереосистема Bluetooth",
      "Возможность сидеть на носу катера"
    ],
  },
  {
    id: 2,
    name: "Бриз 17",
    image: new URL('../assets/images/briz-17.jpg', import.meta.url).href,
    specs: [
      { label: "Длина", value: "5.1 м", icon: "mdi:ruler" },
      { label: "Вместимость", value: "5 человек", icon: "mdi:account-group" },
      { label: "Мощность", value: "70 л.с.", icon: "mdi:engine" }
    ],
    features: [
      "Закрытая кабина",
      "Стереосистема Bluetooth",
      "Открытая корма без тента",
      "Небольшая каюта"
    ],
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

  const section = document.querySelector('.fleet-section')
  if (section) observer.observe(section)
})

const scrollToContact = () => {
  document.querySelector('#contact')?.scrollIntoView({
    behavior: 'smooth'
  })
}
</script>

<template>
  <section id="fleet" class="fleet-section">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">Наш флот</h2>
        <div class="section-divider"></div>
        <p class="section-subtitle">Современные и комфортабельные катера для любых прогулок</p>
      </div>
      
      <div class="fleet-grid">
        <div 
          v-for="(boat, index) in fleet" 
          :key="boat.id" 
          class="boat-card"
          :class="{ 'animate-in': animated }"
          :style="{ '--delay': index * 0.1 + 's' }"
        >
          <div class="boat-image-container">
            <img 
              :src="boat.image" 
              :alt="boat.name" 
              class="boat-image"
              loading="lazy"
            >
            <div class="image-overlay"></div>
          </div>
          
          <div class="boat-content">
            <h3 class="boat-name">{{ boat.name }}</h3>
            
            <div class="specs-grid">
              <div v-for="(spec, index) in boat.specs" :key="index" class="spec-item">
                <div class="spec-icon">
                  <Icon :icon="spec.icon" />
                </div>
                <div class="spec-info">
                  <span class="spec-label">{{ spec.label }}</span>
                  <span class="spec-value">{{ spec.value }}</span>
                </div>
              </div>
            </div>
            
            <div class="features">
              <h4 class="features-title">Особенности:</h4>
              <ul class="features-list">
                <li v-for="(feature, index) in boat.features" :key="index">
                  <Icon icon="mdi:check-circle" class="feature-icon" />
                  <span>{{ feature }}</span>
                </li>
              </ul>
            </div>
            
            <button class="cta-button" @click="scrollToContact">
              <span>Забронировать {{ boat.name }}</span>
              <Icon icon="mdi:arrow-right" class="button-icon" />
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&family=Montserrat:wght@300;400;600&display=swap');

.fleet-section {
  padding: 6rem 0;
  background-color: #bcf9f9;
  position: relative;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
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
  margin: 0 auto 1.5rem;
  border-radius: 2px;
  opacity: 0;
  transform: scaleX(0);
  transform-origin: center;
  animation: scaleIn 0.8s ease-out forwards;
  animation-delay: 0.3s;
}

.section-subtitle {
  font-family: 'Manrope', sans-serif;
  font-size: clamp(1.1rem, 2vw, 1.3rem);
  color: #64748b;
  max-width: 700px;
  margin: 0 auto;
  line-height: 1.6;
  opacity: 0;
  transform: translateY(10px);
  animation: fadeUp 0.8s ease-out forwards;
  animation-delay: 0.6s;
}

.fleet-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
  gap: 2.5rem;
}

.boat-card {
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  opacity: 0;
  transform: translateY(30px);
}

.boat-card.animate-in {
  animation: cardFadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) forwards;
  animation-delay: var(--delay);
}

.boat-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 15px 40px rgba(31, 140, 201, 0.15);
}

.boat-image-container {
  position: relative;
  height: 300px;
  overflow: hidden;
}

.boat-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.8s ease;
}

.image-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.2), transparent 50%);
}

.boat-card:hover .boat-image {
  transform: scale(1.05);
}

.boat-content {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  height: calc(100% - 300px);
}

.boat-name {
  font-family: 'Manrope', sans-serif;
  font-size: 1.4rem;
  font-weight: 700;
  margin: 0 0 1.5rem 0;
  color: #1e293b;
  transition: color 0.3s ease;
}

.boat-card:hover .boat-name {
  color: #3da7cc;
}

.specs-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.spec-item {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  background: #f8fafc;
  padding: 1rem;
  border-radius: 10px;
  transition: transform 0.3s ease;
}

.spec-item:hover {
  transform: translateY(-3px);
}

.spec-icon {
  width: 40px;
  height: 40px;
  background: linear-gradient(135deg, #4fc1e9, #7fdbff);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  flex-shrink: 0;
}

.spec-icon svg {
  width: 20px;
  height: 20px;
}

.spec-info {
  display: flex;
  flex-direction: column;
}

.spec-label {
  font-family: 'Manrope', sans-serif;
  font-size: 0.7rem;
  font-weight: 600;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.spec-value {
  font-family: 'Manrope', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  color: #1e293b;
}

.features {
  margin-top: auto;
}

.features-title {
  font-family: 'Manrope', sans-serif;
  font-size: 1.1rem;
  font-weight: 700;
  margin: 1.5rem 0 1rem 0;
  color: #1e293b;
}

.features-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.features-list li {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.6rem;
  color: #475569;
  font-family: 'Manrope', sans-serif;
}

.feature-icon {
  color: #4fc1e9;
  flex-shrink: 0;
}

.cta-button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.8rem;
  width: 100%;
  padding: 1rem;
  margin-top: 2rem;
  background: linear-gradient(135deg, #4fc1e9, #3da7cc);
  color: white;
  border: none;
  border-radius: 8px;
  font-family: 'Manrope', sans-serif;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  box-shadow: 0 4px 15px rgba(63, 167, 204, 0.3);
}

.cta-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(63, 167, 204, 0.4);
}

.button-icon {
  transition: transform 0.3s ease;
}

.cta-button:hover .button-icon {
  transform: translateX(3px);
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

@media (max-width: 1024px) {
  .fleet-grid {
    grid-template-columns: 1fr;
    max-width: 600px;
    margin: 0 auto;
  }
}

@media (max-width: 768px) {
  .fleet-section {
    padding: 4rem 0;
  }
  
  .specs-grid {
    grid-template-columns: 1fr;
  }
  
  .boat-image-container {
    height: 250px;
  }
}

@media (max-width: 480px) {
  .boat-image-container {
    height: 200px;
  }
  
  .boat-content {
    padding: 1.5rem;
  }
}
</style>