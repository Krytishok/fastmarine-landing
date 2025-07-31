<script setup>
import { ref } from 'vue'

const currentTestimonial = ref(0)

const testimonials = [
  {
    name: 'Анна и Михаил',
    text: 'Прекрасная прогулка на закате! Отличный сервис, внимательный экипаж. Обязательно повторим!',
    date: '15.07.2023'
  },
  {
    name: 'Сергей',
    text: 'Ходили на рыбалку - поймали несколько крупных рыб. Капитан знает лучшие места!',
    date: '22.06.2023'
  },
  {
    name: 'Ольга',
    text: 'Заказывали прогулку для компании друзей - все в восторге! Особенно понравилась остановка для купания.',
    date: '08.05.2023'
  }
]

const nextTestimonial = () => {
  currentTestimonial.value = (currentTestimonial.value + 1) % testimonials.length
}

const prevTestimonial = () => {
  currentTestimonial.value = (currentTestimonial.value - 1 + testimonials.length) % testimonials.length
}
</script>

<template>
  <section id="testimonials" class="testimonials">
    <div class="container">
      <h2>Отзывы наших гостей</h2>
      <div class="testimonial-container">
        <button class="nav-button prev" @click="prevTestimonial">‹</button>
        
        <div class="testimonial-card">
          <p class="testimonial-text">"{{ testimonials[currentTestimonial].text }}"</p>
          <div class="testimonial-author">
            <strong>{{ testimonials[currentTestimonial].name }}</strong>
            <span>{{ testimonials[currentTestimonial].date }}</span>
          </div>
        </div>
        
        <button class="nav-button next" @click="nextTestimonial">›</button>
      </div>
    </div>
  </section>
</template>

<style scoped>
.testimonials {
  padding: 4rem 0;
  background-color: #f9f9f9;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

h2 {
  text-align: center;
  margin-bottom: 3rem;
  font-size: 2rem;
}

.testimonial-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2rem;
}

.testimonial-card {
  max-width: 600px;
  padding: 2rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.testimonial-text {
  font-size: 1.2rem;
  font-style: italic;
  margin-bottom: 1rem;
  color: #333;
}

.testimonial-author {
  display: flex;
  flex-direction: column;
}

.testimonial-author strong {
  color: #2c3e50;
}

.testimonial-author span {
  color: #666;
  font-size: 0.9rem;
}

.nav-button {
  background: #4fc1e9;
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  font-size: 1.5rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s;
}

.nav-button:hover {
  background: #3da7cc;
}

@media (max-width: 768px) {
  .testimonial-container {
    flex-direction: column;
  }
  
  .nav-button {
    order: 1;
  }
}
</style>