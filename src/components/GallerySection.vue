<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const photos = [
  {
    id: 1,
    src: new URL('../assets/images/gallery1.jpg', import.meta.url).href,
    alt: 'Вид на Золотой мост',
    caption: 'Вид на Золотой мост'
  },
  {
    id: 2,
    src: new URL('../assets/images/gallery2.jpg', import.meta.url).href,
    alt: 'Елена',
    caption: 'Птичий базар острова Елены'
  },
  {
    id: 3,
    src: new URL('../assets/images/gallery3.jpg', import.meta.url).href,
    alt: 'Остров Уши',
    caption: 'Остров Уши'
  },
  {
    id: 4,
    src: new URL('../assets/images/gallery4.jpeg', import.meta.url).href,
    alt: "Маяк",
    caption: 'Токаревский маяк'
  }
]

const currentIndex = ref(0)
const animated = ref(false)
let intervalId = null

const nextSlide = () => {
  currentIndex.value = (currentIndex.value + 1) % photos.length
  resetAutoPlay()
}

const prevSlide = () => {
  currentIndex.value = (currentIndex.value - 1 + photos.length) % photos.length
  resetAutoPlay()
}

const goToSlide = (index) => {
  currentIndex.value = index
  resetAutoPlay()
}

const startAutoPlay = () => {
  if (!intervalId) {
    intervalId = setInterval(nextSlide, 5000)
  }
}

const stopAutoPlay = () => {
  if (intervalId) {
    clearInterval(intervalId)
    intervalId = null
  }
}

const resetAutoPlay = () => {
  stopAutoPlay()
  startAutoPlay()
}

onMounted(() => {
  startAutoPlay()
  
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        animated.value = true
        observer.unobserve(entry.target)
      }
    })
  }, { threshold: 0.1 })

  const section = document.querySelector('.gallery')
  if (section) observer.observe(section)
})

onBeforeUnmount(() => {
  stopAutoPlay()
})
</script>

<template>
  <section id="gallery" class="gallery">
    <div class="container">
      <div class="section-header" :class="{ 'animate-in': animated }">
        <h2 class="section-title">Галерея</h2>
        <div class="section-divider"></div>
        <p class="section-subtitle">Увидьте эмоции наших гостей и красоту морских пейзажей</p>
      </div>
      
      <div class="slider-wrapper">
        <div class="slider-container"
             @mouseenter="stopAutoPlay"
             @mouseleave="startAutoPlay">
          <div class="slider" :style="{ transform: 'translateX(-' + currentIndex * 100 + '%)' }">
            <div class="slide" 
                 v-for="(photo, index) in photos" 
                 :key="photo.id"
                 :class="{ active: index === currentIndex }">
              <img 
                :src="photo.src" 
                :alt="photo.alt" 
                class="slide-image"
                loading="lazy"
                :style="{ 'view-transition-name': 'slide-' + index }"
              >
              <div class="slide-overlay"></div>
              <div class="slide-caption">
                <div class="caption-content">
                  <p>{{ photo.caption }}</p>
                </div>
              </div>
            </div>
          </div>
          
          <button class="nav-button prev" @click="prevSlide" aria-label="Previous">
            <Icon icon="mdi:chevron-left" />
          </button>
          <button class="nav-button next" @click="nextSlide" aria-label="Next">
            <Icon icon="mdi:chevron-right" />
          </button>
        </div>
        
        <div class="dots">
          <button 
            v-for="(_, index) in photos" 
            :key="index"
            @click="goToSlide(index)"
            :class="{ active: index === currentIndex }"
            aria-label="Go to slide"
          >
            <span class="dot-progress" v-if="index === currentIndex"></span>
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&family=Montserrat:wght@300;400;600&display=swap');

.gallery {
  padding: 6rem 0;
  background-color: #f8fafc;
  position: relative;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
  opacity: 0;
  transform: translateY(20px);
}

.section-header.animate-in {
  animation: fadeUp 0.8s ease-out forwards;
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

.slider-wrapper {
  max-width: 1200px;
  margin: 0 auto;
}

.slider-container {
  position: relative;
  overflow: hidden;
  border-radius: 16px;
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.1);
}

.slider {
  display: flex;
  transition: transform 0.6s cubic-bezier(0.22, 1, 0.36, 1);
  height: 600px;
}

.slide {
  min-width: 100%;
  position: relative;
}

.slide-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.slide-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.7) 0%, rgba(0, 0, 0, 0) 50%);
}

.slide-caption {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 2.5rem;
  z-index: 2;
}

.caption-content {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}

.slide-caption p {
  font-family: 'Manrope', sans-serif;
  color: white;
  font-size: clamp(1.2rem, 2.5vw, 1.6rem);
  font-weight: 500;
  line-height: 1.5;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  margin: 0;
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.6s cubic-bezier(0.22, 1, 0.36, 1);
}

.slide.active .slide-caption p {
  opacity: 1;
  transform: translateY(0);
  transition-delay: 0.4s;
}

.nav-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: none;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 10;
  transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  backdrop-filter: blur(5px);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.8;
}

.nav-button:hover {
  background: rgba(255, 255, 255, 0.3);
  opacity: 1;
  transform: translateY(-50%) scale(1.1);
}

.nav-button svg {
  width: 30px;
  height: 30px;
}

.prev {
  left: 30px;
}

.next {
  right: 30px;
}

.dots {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
  gap: 12px;
}

.dots button {
  position: relative;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  border: none;
  background: #e2e8f0;
  cursor: pointer;
  padding: 0;
  overflow: hidden;
  transition: background 0.3s;
}

.dots button.active {
  background: #cbd5e1;
}

.dot-progress {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 0;
  background: linear-gradient(90deg, #4fc1e9, #7fdbff);
  animation: dotProgress 5s linear forwards;
}

.dots button:not(.active) .dot-progress {
  display: none;
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

@keyframes dotProgress {
  from {
    width: 0;
  }
  to {
    width: 100%;
  }
}

@media (max-width: 1024px) {
  .slider {
    height: 500px;
  }
  
  .nav-button {
    width: 50px;
    height: 50px;
  }
  
  .nav-button svg {
    width: 25px;
    height: 25px;
  }
}

@media (max-width: 768px) {
  .gallery {
    padding: 4rem 0;
  }
  
  .slider {
    height: 400px;
  }
  
  .slide-caption {
    padding: 1.5rem;
  }
  
  .nav-button {
    width: 40px;
    height: 40px;
  }
  
  .nav-button svg {
    width: 20px;
    height: 20px;
  }
  
  .dots button {
    width: 12px;
    height: 12px;
  }
}

@media (max-width: 480px) {
  .slider {
    height: 300px;
  }
  
  .section-header {
    margin-bottom: 2rem;
  }
}
</style>