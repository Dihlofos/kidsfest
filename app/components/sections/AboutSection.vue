<script setup>
import { sanitizeText } from '~/utils/sanitize'

const props = defineProps({
  title: { type: Array, required: true },
  description: { type: String, default: '' },
  cards: { type: Array, default: () => [] },
  button: { type: Object, default: () => ({}) },
  images: { type: Object, default: () => ({}) },
})
</script>

<template>
  <section id="about" class="about">
    <Image
      v-if="images.background"
      class="about__background"
      :src="images.background"
      alt=""
      width="2316"
      height="1048"
      :img-attrs="{ 'aria-hidden': 'true' }"
    />

    <Container class="about__container">
      <Image
        v-if="images.arcBlue"
        class="about__arc about__arc--blue"
        :src="images.arcBlue"
        alt=""
        width="529"
        height="668"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />
      <Image
        v-if="images.arcGreen"
        class="about__arc about__arc--green"
        :src="images.arcGreen"
        alt=""
        width="640"
        height="753"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />

      <h2 class="about__title">
        <span
          v-for="line in title"
          :key="line"
          class="about__title-line"
          v-html="sanitizeText(line)"
        />
      </h2>

      <p
        v-if="description"
        class="about__description"
        v-html="sanitizeText(description)"
      />

      <div v-if="cards.length" class="about__cards">
        <article
          v-for="card in cards"
          :key="card.title.join(' ')"
          class="about__card"
          :class="[
            `about__card--${card.variant}`,
            `about__card--${card.size}`,
          ]"
        >
          <h3 class="about__card-title">
            <span
              v-for="line in card.title"
              :key="line"
              class="about__card-title-line"
              v-html="sanitizeText(line)"
            />
          </h3>
          <p
            class="about__card-description"
            v-html="sanitizeText(card.description)"
          />
        </article>
      </div>

      <Button
        v-if="button?.text"
        class="about__button"
        :to="button.link"
        variant="black"
      >
        <span v-html="sanitizeText(button.text)" />
      </Button>
    </Container>
  </section>
</template>

<style lang="scss">
.about {
  position: relative;
  z-index: 4;
  margin-top: -5.6rem;
  overflow: visible;
  color: $cyan;

  &::after {
    content: '';
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 0;
    height: 13.2rem;
    background-color: $cyan;
  }

  &__background {
    position: absolute;
    top: 0;
    left: 50%;
    z-index: 1;
    width: max(231.6rem, 100vw);
    max-width: none;
    height: calc(100% + 9.6rem);
    transform: translateX(-50%);
    pointer-events: none;
    user-select: none;
  }

  &__arc {
    position: absolute;
    z-index: 3;
    max-width: none;
    height: auto;
    pointer-events: none;
    user-select: none;
  }

  &__arc--blue {
    top: -2.8rem;
    left: -6.9rem;
    width: 26.5rem;

     @media (max-width: $tablet) {
      display: none;
    }
  }

  &__arc--green {
    right: -5.8rem;
    bottom: -4.3rem;
    width: 32rem;

    @media (max-width: $tablet) {
      display: none;
    }
  }

  &__container {
    position: relative;
    z-index: 2;
    padding-top: 9.4rem;
    padding-bottom: 16rem;

    @media (max-width: $mobile-s) {
      padding-top: 7rem;
    }
  }

  &__title {
    max-width: 90rem;
    margin: 0 auto;
    color: $cyan;
    text-align: center;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(4.4rem, 1, 700);

    @media (max-width: $tablet) {
      max-width: 70rem;
      @include font(4rem, 0.95, 700);
    }

    @media (max-width: $mobile) {
      max-width: 90%;
      font-size: 3.2rem;
    }

    @media (max-width: $mobile-s) {
      font-size: 2.8rem;
    }
  }

  &__title-line,
  &__card-title-line {
    display: block;
  }

  &__description {
    max-width: 94rem;
    margin: 2.2rem auto 0;
    text-align: center;
    font-style: normal;
    @include font(2.4rem, 1.1, 700);

    @media (max-width: $tablet) {
      @include font(2rem, 1.2, 400);
    }

    @media (max-width: $mobile) {
      @include font(1.6rem, 1.25, 400);
    }

    @media (max-width: $mobile-s) {
      margin-top: 1.8rem;
    }
  }

  &__cards {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 2.4rem;
    max-width: 110rem;
    margin: 4.4rem auto 0;

    @media (max-width: $mobile-s) {
      gap: 2rem;
      margin-top: 3.2rem;
    }
  }

  &__card {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 48.6rem;
    min-height: 14.7rem;
    padding: 2rem;
    border-radius: 28px;
    color: $white;
    text-align: center;

    @media (max-width: $tablet) {
      width: calc(50% - 1.2rem);
    }

    @media (max-width: $mobile) {
      width: 100%;
      min-height: 13rem;
      padding: 1.8rem;
    }
  }

  &__card--wide {
    width: 58.8rem;

    @media (max-width: $tablet) {
      width: calc(50% - 1.2rem);
    }

    @media (max-width: $mobile) {
      width: 100%;
    }
  }

  &__card--turquoise {
    background-color: $turquoise;
  }

  &__card--green {
    background-color: $green;
  }

  &__card-title {
    margin: 0;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(3.2rem, 1, 900);

    @media (max-width: $tablet) {
      @include font(2.8rem, 1, 700);
    }

    @media (max-width: $mobile) {
      font-size: 2.4rem;
    }

    @media (max-width: $mobile-s) {
      font-size: 2.2rem;
    }
  }

  &__card-description {
    margin: 1rem 0 0;
    font-style: normal;
    @include font(2rem, 1.1, 400);

    @media (max-width: $mobile) {
      @include font(1.6rem, 1.2, 400);
    }
  }

  &__button {
    display: flex;
    width: 32.1rem;
    margin: 4rem auto 0;

    @media (max-width: $mobile) {
      width: 100%;
      min-width: 100%;
    }

    @media (max-width: $mobile) {
      width: 30rem;
      max-width: 100%;
      min-width: 0;
      height: 5.2rem;
      padding: 0 2.4rem;
      font-size: 1.7rem;
   }
  }

  @media (hover: hover) {
    &__button:not(:disabled):hover {
      background-color: $green;
    }
  }
}
</style>
