<script setup>
import { ref, onMounted } from 'vue'

const tours = ref([
  {
    id: 1,
    title: 'Вокруг острова Елены',
    description: 'Незабываемое путешествие вокруг островов Уши и Елена. Самая популярная прогулка',
    price: 6000,
    image: new URL('../assets/images/ushi.jpg', import.meta.url).href,
    routeURL: 'https://www.google.com/maps/d/u/0/embed?mid=1bsvUle3jB9E_um53ehJj1HSTakVBARM&ehbc=2E312F&noprof=1',
    showRoute: false,
    isLoading: false,
    routeInfo: {
      points: [
        'Старт: Причал яхт-клуба',
        'Канал',
        'Остров Уши - фотостоп',
        'Птичий базар о. Елены',
        'Токаревский маяк',
      ],
      duration: '30 минут'
    }
  },
  {
    id: 2,
    title: 'Под Золотой мост',
    description: 'Невероятное путешествие в самое сердце города. Прекрасные виды на мост и на центр города Владивосток',
    price: 6000,
    image: new URL('../assets/images/golden-bridge.jpg', import.meta.url).href,
    routeURL: 'https://www.google.com/maps/d/u/0/embed?mid=1WDe4Ad9oWvrOjHn76ik3fam8KmLKcJc&ehbc=2E312F&noprof=1',
    showRoute: false,
    isLoading: false,
    routeInfo: {
      points: [
        'Старт: Причал яхт-клуба',
        'Золотой Рог',
        'Золотой Мост',
        'Набережная Цесаревича'
      ],
      duration: '30 минут'
    }
  },
  {
    id: 3,
    title: 'Под Русский мост',
    description: 'Фантастическая прогулка под самый красивый мост Приморского края, виды на о. Скрыплева и ДВФУ',
    price: 6000,
    image: new URL('../assets/images/russky-bridge.webp', import.meta.url).href,
    routeURL: 'https://www.google.com/maps/d/u/0/embed?mid=19EUReEy_MmANl1jkHjUcao3V0ppOMCs&ehbc=2E312F&noprof=1',
    showRoute: false,
    isLoading: false,
    routeInfo: {
      points: [
        'Старт: Причал яхт-клуба',
        'Пролив Босфор-Восточный',
        'Русский мост',
        'ДВФУ',
      ],
      duration: '30 минут'
    }
  }
])

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

  const section = document.querySelector('.tours')
  if (section) observer.observe(section)
})

const toggleRoute = async (tour) => {
  // Закрываем все другие карточки перед открытием текущей
  tours.value.forEach(t => {
    if (t.id !== tour.id) {
      t.showRoute = false
      t.isLoading = false
    }
  })
  
  tour.showRoute = !tour.showRoute
  
  if (tour.showRoute) {
    tour.isLoading = true
    await new Promise(resolve => setTimeout(resolve, 700))
    tour.isLoading = false
  }
}

const scrollToContact = () => {
  document.querySelector('#contact')?.scrollIntoView({
    behavior: 'smooth'
  })
}
</script>

<template>
  <section id="tours" class="tours">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">Наши прогулки</h2>
        <div class="section-divider"></div>
      </div>
      
      <div class="tours-grid">
        <div 
          class="tour-card-wrapper"
          v-for="(tour, index) in tours" 
          :key="tour.id"
          :class="{ 'animate-in': animated }"
          :style="{ '--delay': index * 0.1 + 's' }"
        >
          <div class="tour-card">
            <div class="tour-image-wrapper">
              <img :src="tour.image" :alt="tour.title" class="tour-image">
              <div class="price-badge">
                от {{ tour.price }} ₽
                <div class="badge-wave"></div>
              </div>
            </div>
            
            <div class="card-content">
              <h3 class="tour-title">{{ tour.title }}</h3>
              <p class="tour-description">{{ tour.description }}</p>
              
              <div class="buttons">
                <button class="book-button" @click="scrollToContact">
                  <span>Забронировать</span>
                  <Icon icon="mdi:arrow-right" class="button-icon" />
                </button>
                <button 
                  class="route-button" 
                  @click="toggleRoute(tour)"
                  :class="{ active: tour.showRoute }"
                >
                  <span>{{ tour.showRoute ? 'Скрыть маршрут' : 'Узнать маршрут' }}</span>
                  <Icon :icon="tour.showRoute ? 'mdi:chevron-up' : 'mdi:map-marker-path'" class="button-icon" />
                </button>
              </div>
              
              <transition name="route">
                <div v-if="tour.showRoute" class="route-map">
                  <div class="map-container">
                    <div v-if="tour.isLoading" class="map-loading">
                      <div class="loading-spinner">
                        <div class="spinner-wave"></div>
                        <div class="spinner-wave"></div>
                        <div class="spinner-wave"></div>
                        <div class="spinner-wave"></div>
                      </div>
                      <div class="loading-text">Загрузка карты...</div>
                    </div>
                    
                    <iframe 
                      v-show="!tour.isLoading"
                      :src="tour.routeURL" 
                      class="google-map"
                      loading="lazy"
                      allowfullscreen
                      referrerpolicy="no-referrer-when-downgrade"
                    ></iframe>
                  </div>
                  
                  <div class="route-info">
                    <h4 class="route-title">Точки маршрута:</h4>
                    <ul class="route-points">
                      <li v-for="(point, index) in tour.routeInfo.points" :key="index">
                        <span class="point-number">{{ index + 1 }}</span>
                        {{ point }}
                      </li>
                    </ul>
                    <div class="duration-badge">
                      <Icon icon="mdi:clock-outline" />
                      <span>{{ tour.routeInfo.duration }}</span>
                    </div>
                  </div>
                </div>
              </transition>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&family=Montserrat:wght@300;400;600&display=swap');

.tours {
  padding: 6rem 0;
  background-color: #ddf6ff;
  position: relative;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  position: relative;
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

.tours-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2.5rem;
  align-items: start;
}

.tour-card-wrapper {
  position: relative;
}

.tour-card-wrapper.animate-in {
  animation: cardFadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) forwards;
  animation-delay: var(--delay);
}

.tour-card {
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  opacity: 0;
  transform: translateY(30px);
  height: 100%;
  display: flex;
  flex-direction: column;
}

.tour-card-wrapper.animate-in .tour-card {
  opacity: 1;
  transform: translateY(0);
}

.tour-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 15px 40px rgba(31, 140, 201, 0.15);
}

.tour-image-wrapper {
  position: relative;
  height: 250px;
  overflow: hidden;
}

.tour-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.8s ease;
}

.tour-card:hover .tour-image {
  transform: scale(1.05);
}

.price-badge {
  position: absolute;
  bottom: 20px;
  right: 20px;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 0.6rem 1.2rem;
  border-radius: 30px;
  font-weight: 700;
  font-size: 1.2rem;
  z-index: 2;
  overflow: hidden;
}

.badge-wave {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 3px;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.6s cubic-bezier(0.23, 1, 0.32, 1);
}

.tour-card:hover .badge-wave {
  transform: scaleX(1);
}

.card-content {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.tour-title {
  font-family: 'Manrope', sans-serif;
  font-size: 1.4rem;
  font-weight: 700;
  margin: 0 0 1rem 0;
  color: #1e293b;
  transition: color 0.3s ease;
}

.tour-card:hover .tour-title {
  color: #3da7cc;
}

.tour-description {
  font-family: 'Manrope', sans-serif;
  color: #64748b;
  margin-bottom: 1.5rem;
  line-height: 1.6;
  flex-grow: 1;
}

.buttons {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

.book-button, .route-button {
  flex: 1;
  padding: 0.8rem;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.book-button {
  background: linear-gradient(135deg, #4fc1e9, #3da7cc);
  color: white;
  box-shadow: 0 4px 15px rgba(63, 167, 204, 0.3);
}

.book-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(63, 167, 204, 0.4);
}

.route-button {
  background-color: #f1f5f9;
  color: #334155;
  border: 1px solid #e2e8f0;
}

.route-button:hover, .route-button.active {
  background-color: #e2e8f0;
  color: #1e293b;
}

.button-icon {
  font-size: 1.2rem;
  transition: transform 0.3s ease;
}

.book-button:hover .button-icon {
  transform: translateX(3px);
}

.route-enter-active,
.route-leave-active {
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  overflow: hidden;
}

.route-enter-from,
.route-leave-to {
  opacity: 0;
  max-height: 0;
  margin-top: 0;
}

.route-enter-to,
.route-leave-from {
  opacity: 1;
  max-height: 1000px;
  margin-top: 1.5rem;
}

.route-map {
  margin-top: 1.5rem;
  padding-top: 1.5rem;
  border-top: 1px solid #f1f5f9;
}

.map-container {
  position: relative;
  width: 100%;
  height: 300px;
  padding-bottom: 56.25%;
  overflow: hidden;
  border-radius: 12px;
  margin-bottom: 1.5rem;
  background-color: #f1f5f9;
}

.google-map {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border: none;
}

.map-loading {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #f8fafc;
  border-radius: 12px;
}

.loading-spinner {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  height: 50px;
  margin-bottom: 1rem;
  gap: 5px;
}

.spinner-wave {
  width: 8px;
  height: 30px;
  background: linear-gradient(to top, #4fc1e9, #7fdbff);
  border-radius: 4px;
  animation: spinner-wave 1.2s ease-in-out infinite;
}

.spinner-wave:nth-child(1) {
  animation-delay: -0.4s;
}

.spinner-wave:nth-child(2) {
  animation-delay: -0.3s;
}

.spinner-wave:nth-child(3) {
  animation-delay: -0.2s;
}

.spinner-wave:nth-child(4) {
  animation-delay: -0.1s;
}

.loading-text {
  color: #64748b;
  font-weight: 500;
  font-family: 'Manrope', sans-serif;
}

.route-info {
  background: #f8fafc;
  padding: 1.5rem;
  border-radius: 12px;
  position: relative;
}

.route-title {
  font-family: 'Manrope', sans-serif;
  font-size: 1.1rem;
  font-weight: 700;
  margin: 0 0 1rem 0;
  color: #1e293b;
}

.route-points {
  list-style: none;
  padding: 0;
  margin: 0 0 1rem 0;
}

.route-points li {
  position: relative;
  padding-left: 2rem;
  margin-bottom: 0.8rem;
  color: #475569;
  line-height: 1.5;
}

.point-number {
  position: absolute;
  left: 0;
  top: 0;
  width: 1.5rem;
  height: 1.5rem;
  background: linear-gradient(135deg, #4fc1e9, #7fdbff);
  color: white;
  border-radius: 50%;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
  font-weight: 700;
}

.duration-badge {
  position: absolute;
  top: -12px;
  right: 20px;
  background: linear-gradient(135deg, #4fc1e9, #7fdbff);
  color: white;
  padding: 0.4rem 1.2rem;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 5px;
  box-shadow: 0 4px 12px rgba(79, 193, 233, 0.3);
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

@keyframes spinner-wave {
  0%, 100% {
    height: 10px;
  }
  50% {
    height: 30px;
  }
}

@media (max-width: 768px) {
  .tours {
    padding: 4rem 0;
  }
  
  .tours-grid {
    grid-template-columns: 1fr;
  }
  
  .buttons {
    flex-direction: column;
  }
  
  .tour-image-wrapper {
    height: 220px;
  }
  
  .price-badge {
    font-size: 1rem;
    padding: 0.5rem 1rem;
  }
}
</style>