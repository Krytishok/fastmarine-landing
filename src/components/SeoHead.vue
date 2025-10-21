<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  // Устанавливаем мета-теги
  document.title = 'Морские прогулки во Владивостоке | Аренда катера | ФастМарин (FastMarine)'
  
  // Description
  let metaDescription = document.querySelector('meta[name="description"]')
  if (!metaDescription) {
    metaDescription = document.createElement('meta')
    metaDescription.name = 'description'
    document.head.appendChild(metaDescription)
  }
  metaDescription.content = 'Фастмарин (FastMarine) - Морские прогулки на катере во Владивостоке. Аренда катера, экскурсии по Амурскому заливу, проливу Босфор-Восточный. Доступные цены, опытный капитан.'
  
  // Keywords
  let metaKeywords = document.querySelector('meta[name="keywords"]')
  if (!metaKeywords) {
    metaKeywords = document.createElement('meta')
    metaKeywords.name = 'keywords'
    document.head.appendChild(metaKeywords)
  }
  metaKeywords.content = 'фастмарин, fastmarine, морские прогулки Владивосток, аренда катера, экскурсии по заливу, аренда яхты Владивосток'
  
  // Structured Data с названием компании
  const structuredData = {
    "@context": "https://schema.org",
    "@type": "TouristAttraction", 
    "name": "Фастмарин (FastMarine) - Морские прогулки",
    "alternateName": "FastMarine",
    "description": "Аренда катера для морских прогулок во Владивостоке",
    "address": {
      "@type": "PostalAddress",
      "addressLocality": "Владивосток",
      "addressCountry": "RU"
    }
  }
  
  const script = document.createElement('script')
  script.type = 'application/ld+json'
  script.text = JSON.stringify(structuredData)
  document.head.appendChild(script)
})
</script>

<template>
  <!-- Пустой компонент -->
</template>