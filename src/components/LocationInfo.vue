<script setup>
import { onMounted, ref } from 'vue'

const locationCards = [
  {
    title: "Яхт-клуб",
    image: "view.jpg",
    description: "Находится левее Токаревского маяка, если смотреть со стороны парковки"
  },
  {
    title: "Наш баннер",
    image: "banner_orient.jpg",
    description: "Именно здесь мы и располагаемся, если никого нет на месте, позвоните по номеру телефона на баннере"
  },
  {
    title: "Пирс",
    image: "flags.jpg",
    description: "Отсюда будет начинаться наше путешествие"
  }
]

const animatedCards = ref(Array(locationCards.length).fill(false))

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const index = parseInt(entry.target.dataset.index)
        animatedCards.value[index] = true
      }
    })
  }, {
    threshold: 0.1,
    rootMargin: '0px 0px -50px 0px'
  })

  document.querySelectorAll('.location-card').forEach((card, index) => {
    card.dataset.index = index
    observer.observe(card)
  })
})
</script>

<template>
  <section id="location" class="location-info">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">Как добраться</h2>
        <div class="title-line"></div>
      </div>
      
      <div class="location-cards">
        <div 
          v-for="(item, index) in locationCards" 
          :key="index" 
          class="location-card"
          :class="{ 'animated': animatedCards[index] }"
        >
          <div class="image-wrapper">
            <img 
              :src="`/src/assets/images/${item.image}`" 
              :alt="item.title"
              class="location-image"
              loading="lazy"
            >
            <div class="image-overlay"></div>
          </div>
          <div class="card-content">
            <h3>{{ item.title }}</h3>
            <div class="divider"></div>
            <p>{{ item.description }}</p>
          </div>
        </div>
      </div>
      
      <div class="cta-block">
        <h3 class="cta-title">Приходите и отправляйтесь в море!</h3>
        <div class="contact-info">
          <div class="contact-item">
            <span class="icon">📍</span>
            <p>Токаревский маяк, 1 (Яхт-клуб)</p>
          </div>
          <div class="contact-item">
            <span class="icon">⏰</span>
            <p>Ежедневно с 8:00 до 20:00</p>
          </div>
          <a href="tel:+79089920649" class="phone-link">
            <span class="icon">📞</span>
            <span>+7 (908) 992-06-49</span>
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&family=Montserrat:wght@300;400;600&display=swap');

.location-info {
  padding: 6rem 0;
  background: linear-gradient(135deg, #f8fcff 0%, #e6f2ff 100%);
  position: relative;
  overflow: hidden;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  position: relative;
  z-index: 2;
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
  overflow: hidden;
}

.section-title {
  font-family: 'Montserrat', sans-serif;
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 800;
  color: #2c3e50;
  margin-bottom: 1rem;
  opacity: 0;
  transform: translateY(30px);
  animation: fadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) forwards;
}

.title-line {
  height: 4px;
  width: 0;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  margin: 0 auto;
  border-radius: 2px;
  animation: lineReveal 1s ease-out forwards;
  animation-delay: 0.5s;
}

.location-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2.5rem;
  margin-bottom: 4rem;
}

.location-card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  opacity: 0;
  transform: translateY(30px);
}

.location-card.animated {
  animation: fadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) forwards;
}

.location-card:nth-child(1).animated {
  animation-delay: 0.2s;
}
.location-card:nth-child(2).animated {
  animation-delay: 0.4s;
}
.location-card:nth-child(3).animated {
  animation-delay: 0.6s;
}

.location-card:hover {
  transform: translateY(-10px) !important;
  box-shadow: 0 15px 40px rgba(31, 140, 201, 0.15);
}

.image-wrapper {
  height: 280px;
  overflow: hidden;
  position: relative;
}

.image-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to bottom, rgba(0,0,0,0.1), rgba(0,48,96,0.3));
  opacity: 0.7;
  transition: opacity 0.5s;
}

.location-card:hover .image-overlay {
  opacity: 0.4;
}

.location-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.8s cubic-bezier(0.23, 1, 0.32, 1);
}

.location-card:hover .location-image {
  transform: scale(1.1);
}

.card-content {
  padding: 1.8rem;
}

.card-content h3 {
  color: #2c3e50;
  font-family: 'Manrope', sans-serif;
  font-weight: 800;
  font-size: 1.4rem;
  margin-bottom: 1rem;
  position: relative;
}

.divider {
  height: 2px;
  width: 50px;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  margin: 1rem 0;
  border-radius: 1px;
}

.card-content p {
  color: #555;
  line-height: 1.6;
  font-size: 1rem;
  font-family: 'Manrope', sans-serif;
  font-weight: 400;
}

.cta-block {
  text-align: center;
  padding: 3rem 2rem;
  background: linear-gradient(135deg, rgba(79, 193, 233, 0.1) 0%, rgba(127, 219, 255, 0.1) 100%);
  border-radius: 12px;
  max-width: 800px;
  margin: 0 auto;
  backdrop-filter: blur(5px);
  border: 1px solid rgba(255,255,255,0.2);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  opacity: 0;
  transform: translateY(30px);
  animation: fadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) forwards;
  animation-delay: 0.8s;
}

.cta-title {
  font-family: 'Montserrat', sans-serif;
  font-size: clamp(1.5rem, 3vw, 2rem);
  font-weight: 800;
  color: #2c3e50;
  margin-bottom: 1.5rem;
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 2rem;
}

.contact-item {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  font-family: 'Manrope', sans-serif;
  font-size: 1.1rem;
  color: #2c3e50;
}

.icon {
  font-size: 1.2rem;
}

.phone-link {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  margin-top: 1rem;
  color: #4fc1e9;
  font-weight: 700;
  text-decoration: none;
  font-size: 1.2rem;
  transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
  padding: 0.8rem 1.5rem;
  border-radius: 50px;
  background: rgba(255,255,255,0.8);
}

.phone-link:hover {
  color: white;
  background: #4fc1e9;
  transform: translateY(-3px);
  box-shadow: 0 10px 20px rgba(79, 193, 233, 0.3);
}

/* Анимации */
@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes lineReveal {
  from {
    width: 0;
  }
  to {
    width: 150px;
  }
}

@media (max-width: 768px) {
  .location-info {
    padding: 4rem 0;
  }
  
  .location-cards {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .cta-block {
    padding: 2rem 1.5rem;
  }
  
  .contact-item, .phone-link {
    font-size: 1rem;
  }
}

@media (max-width: 480px) {
  .section-title {
    font-size: 2rem;
  }
  
  .cta-title {
    font-size: 1.5rem;
  }
  
  .phone-link {
    padding: 0.6rem 1.2rem;
  }
}
</style>