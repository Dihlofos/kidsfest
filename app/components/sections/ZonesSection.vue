<script setup>
import { computed } from 'vue'
import { sanitizeText } from '~/utils/sanitize'

const props = defineProps({
  title: { type: String, required: true },
  subtitle: { type: Array, default: () => [] },
  cards: { type: Array, default: () => [] },
  images: { type: Object, default: () => ({}) },
})

const showDesktopDecor = computed(() => props.cards.length % 3 === 1)

function getTabletVariant(index) {
  const position = index % 4

  return position === 0 || position === 3
    ? 'green'
    : 'turquoise'
}
</script>

<template>
  <section id="zones" class="zones">
    <Container class="zones__container">
      <h2 class="zones__title" v-html="sanitizeText(title)" />

      <p v-if="subtitle.length" class="zones__subtitle">
        <span
          v-for="line in subtitle"
          :key="line"
          class="zones__subtitle-line"
          v-html="sanitizeText(line)"
        />
      </p>

      <div v-if="cards.length" class="zones__grid">
        <ZoneCard
          v-for="(card, index) in cards"
          :key="card.id"
          :item="card"
          :tablet-variant="getTabletVariant(index)"
        />
      </div>

      <div v-if="cards.length" class="zones__slider">
        <ZoneCard
          v-for="card in cards"
          :key="card.id"
          class="zones__slide"
          :item="card"
        />
      </div>

      <Image
        v-if="images.decor && showDesktopDecor"
        class="zones__decor"
        :src="images.decor"
        alt=""
        width="1597"
        height="717"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />
    </Container>
  </section>
</template>

<style lang="scss">
.zones {
  position: relative;
  z-index: 2;
  padding: 6.6rem 0 18.5rem;
  overflow: hidden;
  background-color: $white;
  color: $black;

  @media (max-width: $tablet) {
    padding: 8rem 0 14.5rem;
  }

  @media (max-width: $mobile) {
      padding: 7rem 0 12.5rem;
  }

  &__container {
    position: relative;
  }

  &__title {
    margin: 0;
    text-align: center;
    text-transform: uppercase;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(6.8rem, 1, 700);

    @media (max-width: $tablet) {
      font-size: 5.2rem;
    }

    @media (max-width: $mobile) {
      font-size: 3.8rem;
    }

    @media (max-width: $mobile-s) {
      font-size: 3.2rem;
    }
  }

  &__subtitle {
    margin: 3.4rem auto 0;
    text-align: center;
    font-style: normal;
    @include font(2rem, 1.3, 700);

    @media (max-width: $mobile) {
      margin-top: 2.4rem;
      font-size: 1.7rem;
    }
  }

  &__subtitle-line {
    display: block;
  }

  &__grid {
    position: relative;
    z-index: 2;
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 2.8rem;
    margin-top: 7rem;

    @media (max-width: $tablet) {
      grid-template-columns: repeat(2, minmax(0, 1fr));
      margin-top: 5.6rem;
    }

    @media (max-width: $mobile) {
      display: none;
    }
  }

  &__slider {
    display: none;

    @media (max-width: $mobile) {
      display: flex;
      gap: 1.2rem;
      margin: 4rem calc(-1 * var(--container-padding, 2rem)) 0;
      padding: 0 var(--container-padding, 2rem);
      overflow-x: auto;
      scroll-behavior: smooth;
      scrollbar-width: none;
      -webkit-overflow-scrolling: touch;

      &::-webkit-scrollbar {
        display: none;
      }
    }
  }

  &__slide {
    @media (max-width: $mobile) {
      flex: 0 0 28rem;
      scroll-snap-align: start;
    }
  }

  &__decor {
    position: absolute;
    right: -4rem;
    bottom: -8rem;
    z-index: 1;
    display: block;
    width: 80rem;
    max-width: none;
    height: auto;
    pointer-events: none;
    user-select: none;

    @media (max-width: $tablet) {
      display: none;
    }
  }
}
</style>
