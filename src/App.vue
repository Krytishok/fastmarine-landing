<script setup>
import Header from './components/Header.vue'
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import ToursSection from './components/ToursSection.vue'
import Testimonials from './components/Testimonials.vue'
import ContactForm from './components/ContactForm.vue'
import Footer from './components/Footer.vue'
import LocationInfo from './components/LocationInfo.vue'
import GallerySection from './components/GallerySection.vue'
import FleetSection from './components/FleetSection.vue'
import WaveDivider from './components/WaveDivider.vue'
import ContactSection from './components/ContactSection.vue'
</script>

<template>
  <div class="app">
    <Header />
    <main>
      <HeroSection />
      <AboutSection />
      <ToursSection />
      <GallerySection />
      <FleetSection />
      <ContactSection />
      <LocationInfo />
    </main>
    <Footer />
  </div>
</template>

<style>
main{
  margin: 0;
  padding: 0;
}

@import './styles/main.css';
.app {
  width: 100%;
  margin: 0;
  padding: 0;
}
</style>