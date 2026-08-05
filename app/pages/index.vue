<script setup>
import heroData from '~/../data/sections/hero.json'
import aboutData from '~/../data/sections/about.json'
import eventsData from '~/../data/sections/events.json'
import zonesData from '~/../data/sections/zones.json'
import activitiesData from '~/../data/sections/activities.json'
import mapData from '~/../data/sections/map.json'
import faqData from '~/../data/sections/faq.json'

// Здесь можно переписать title и description для каждой страницы, если нужно
useHead({
  title: ''
})
</script>

<template>
  <main>
    <Hero v-bind="heroData" />
    <About v-bind="aboutData" />
    <Events v-bind="eventsData" />
    <Zones v-bind="zonesData" />
    <Activities v-bind="activitiesData" />
    <Map v-bind="mapData" />
    <Faq :title="faqData.title" :items="faqData.items" :images="faqData.images" />
  </main>
</template>
