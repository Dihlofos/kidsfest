<script setup>
import { sanitizeText } from '~/utils/sanitize'

defineProps({
  title: { type: String, required: true },
  cards: { type: Array, default: () => [] },
  images: { type: Object, default: () => ({}) },
})

function getTabletVariant(index) {
  const position = index % 4

  return position === 0 || position === 3
    ? 'cyan'
    : 'white'
}
</script>

<template>
  <section id="activities" class="activities">
    <Image
      v-if="images.background"
      class="activities__background"
      :src="images.background"
      alt=""
      width="1440"
      height="1236"
      :img-attrs="{ 'aria-hidden': 'true' }"
    />

    <Container class="activities__container">
      <Image
        v-if="images.decorLeft"
        class="activities__decor activities__decor--left"
        :src="images.decorLeft"
        alt=""
        width="895"
        height="1143"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />

      <Image
        v-if="images.decorRight"
        class="activities__decor activities__decor--right"
        :src="images.decorRight"
        alt=""
        width="1153"
        height="1347"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />

      <h2 class="activities__title" v-html="sanitizeText(title)" />

      <div v-if="cards.length" class="activities__grid">
        <ActivityCard
          v-for="(card, index) in cards"
          :key="card.id"
          :item="card"
          :tablet-variant="getTabletVariant(index)"
        />
      </div>

      <Slider
        v-if="cards.length"
        class="activities__slider"
        :items="cards"
        :arrow-left="images.sliderArrowLeft"
        :arrow-right="images.sliderArrowRight"
        :desktop-breakpoint="744"
      >
        <template #slide="{ item }">
          <ActivityCard :item="item" />
        </template>
      </Slider>
    </Container>
  </section>
</template>

<style lang="scss">
.activities {
  position: relative;
  z-index: 3;
  margin-top: -8rem;
  padding: 9.5rem 0 13rem;
  overflow: visible;
  background-color: $white;
  color: $white;

  @media (max-width: $tablet) {
    margin-top: -6rem;
    padding-top: 13rem;
    padding-bottom: 11rem;
  }

  @media (max-width: $mobile) {
    margin-top: -4rem;
    padding-top: 10rem;
    padding-bottom: 8rem;
  }

  &__background {
    position: absolute;
    top: 0;
    left: 50%;
    z-index: 0;
    width: 100vw;
    max-width: none;
    height: 100%;
    object-fit: fill;
    transform: translateX(-50%);
    pointer-events: none;
    user-select: none;

    @media (max-width: $tablet) {
      width: 120rem;
    }
  }

  &__container {
    position: relative;
    z-index: 1;
  }

  &__decor {
    position: absolute;
    z-index: 0;
    max-width: none;
    height: auto;
    pointer-events: none;
    user-select: none;

    @media (max-width: $mobile) {
      display: none;
    }
  }

  &__decor--left {
    top: -18rem;
    left: -27rem;
    width: 42rem;

    @media (max-width: $tablet) {
      display: none;
    }
  }

  &__decor--right {
    right: -23rem;
    bottom: -19rem;
    width: 62rem;

    @media (max-width: $tablet) {
     display: none;
    }
  }

  &__title {
    position: relative;
    z-index: 2;
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

  &__grid {
    position: relative;
    z-index: 2;
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 2.8rem;
    margin-top: 9rem;

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
      display: block;
      margin-top: 4rem;
      --slider-slide-width: 28rem;
    }
  }
}
</style>
