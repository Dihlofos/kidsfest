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
    <HeroSection v-bind="heroData" />
    <AboutSection v-bind="aboutData" />
    <EventsSection v-bind="eventsData" />
    <ZonesSection v-bind="zonesData" />
    <ActivitiesSection v-bind="activitiesData" />
    <MapSection v-bind="mapData" />
    <FaqSection
      :title="faqData.title"
      :items="faqData.items"
      :images="faqData.images"
    />
  </main>
</template>
