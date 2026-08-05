<script setup>
import { sanitizeText } from '~/utils/sanitize'

const props = defineProps({
  title: { type: String, required: true },
  items: { type: Array, default: () => [] },
  images: { type: Object, default: () => ({}) },
})
</script>

<template>
  <section id="events" class="events">
    <Container class="events__container">
      <Image
        v-if="images.decor"
        class="events__decor"
        :src="images.decor"
        alt=""
        width="1234"
        height="1622"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />

      <h2 class="events__title" v-html="sanitizeText(title)" />

      <div v-if="items.length" class="events__stage">
        <article
          v-for="item in items"
          :key="item.id"
          class="events__item"
          :class="`events__item--${item.id}`"
        >
          <div
            class="events__visual"
            :class="`events__visual--${item.id}`"
          >
            <Image
              class="events__frame"
              :class="`events__frame--${item.id}`"
              :src="item.frame"
              alt=""
              :width="item.frameWidth"
              :height="item.frameHeight"
              :img-attrs="{ 'aria-hidden': 'true' }"
            />

            <div
              class="events__photo"
              :class="`events__photo--${item.id}`"
            >
              <Image
                class="events__photo-image"
                :src="item.photo"
                :alt="item.photoAlt"
                width="850"
                height="850"
              />
            </div>

            <Image
              class="events__frame-piece"
              :class="`events__frame-piece--${item.id}`"
              :src="item.framePiece"
              alt=""
              :width="item.framePieceWidth"
              :height="item.framePieceHeight"
              :img-attrs="{ 'aria-hidden': 'true' }"
            />
          </div>

          <div
            class="events__info"
            :class="[
              `events__info--${item.id}`,
              `events__info--${item.variant}`,
            ]"
          >
            <h3 class="events__item-title">
              <span
                v-for="line in item.title"
                :key="line"
                class="events__item-title-line"
                v-html="sanitizeText(line)"
              />
            </h3>
            <p
              class="events__description"
              v-html="sanitizeText(item.description)"
            />
          </div>
        </article>
      </div>
    </Container>
  </section>
</template>

<style lang="scss">
.events {
  position: relative;
  z-index: 3;
  margin-top: -8.8rem;
  padding-top: 10.8rem;
  overflow: hidden;
  background: linear-gradient(180deg, #1E94B9 37.96%, #6EBFDD 122.33%);
  color: $white;

  @media (max-width: $tablet) {
    min-height: 105rem;
    margin-top: -6.4rem;
    padding-top: 9rem;
    border-radius: 50% 50% 0 0 / 6.4rem 6.4rem 0 0;
  }

  @media (max-width: $mobile) {
    min-height: 0;
    margin-top: -3.6rem;
    padding-top: 7rem;
    padding-bottom: 10rem;
    border-radius: 50% 50% 0 0 / 3.6rem 3.6rem 0 0;
  }

  &__decor {
    position: absolute;
    bottom: -40rem;
    left: -32rem;
    z-index: 0;
    width: 61.6rem;
    max-width: none;
    height: auto;
    pointer-events: none;
    user-select: none;

    @media (max-width: $tablet) {
      bottom: -24rem;
      width: 52rem;
    }

    @media (max-width: $mobile) {
      display: none;
    }
  }

  &__container {
    position: relative;
    z-index: 1;
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

  &__stage {
    position: relative;
    height: 91rem;
    margin-top: 3rem;

    @media (max-width: $tablet) {
      height: 87rem;
      margin-top: 3rem;
    }

    @media (max-width: $mobile) {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 1rem;
      height: auto;
      margin-top: 3rem;
    }
  }

  &__item {
    position: absolute;
    width: 90rem;
    height: 62rem;

    @media (max-width: $tablet) {
      width: 72rem;
      height: 49.6rem;
    }

    @media (max-width: $mobile) {
      position: relative;
      flex: 0 0 auto;
      width: 35.5rem;
      max-width: 100%;
      height: 39rem;
      margin: 0 auto;
    }

    @media (max-width: $xs) {
      height: 35.8rem;
    }
  }

  &__item--first {
    top: 0;
    left: -8.4rem;

    @media (max-width: $tablet) {
      left: -7rem;
    }

    @media (max-width: $mobile) {
      top: auto;
      left: auto;
    }
  }

  &__item--second {
    top: 25rem;
    right: -7.5rem;
    width: 84rem;

    @media (max-width: $tablet) {
      top: 37rem;
      right: -4rem;
      width: 68rem;
    }

    @media (max-width: $mobile) {
      top: auto;
      right: auto;
      width: 35.5rem;
      max-width: 100%;
    }
  }

  &__visual {
    display: contents;

    @media (max-width: $mobile) {
      display: block;
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      aspect-ratio: 1;
    }
  }

  &__visual--second {
    @media (max-width: $mobile) {
      right: 0;
      left: auto;
      width: 96%;
    }
  }

  &__frame,
  &__photo,
  &__frame-piece {
    position: absolute;
    max-width: none;
    height: auto;
    pointer-events: none;
    user-select: none;
  }

  &__frame {
    z-index: 1;
  }

  &__frame--first {
    top: 0;
    left: 0;
    width: 68%;

    @media (max-width: $mobile) {
      left: 0;
      width: 100%;
      max-width: none;
      transform: none;
    }
  }

  &__frame--second {
    top: 0;
    right: 0;
    width: 69.3%;

    @media (max-width: $mobile) {
      right: 0;
      width: 100%;
      max-width: none;
      transform: none;
    }
  }

  &__photo {
    z-index: 2;
    width: 47.2%;
    aspect-ratio: 1;
    overflow: hidden;
    border-radius: 50%;
    clip-path: circle(50% at 50% 50%);
  }

  &__photo-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  &__photo--first {
    top: 10.8%;
    left: 13.6%;

    @media (max-width: $mobile) {
        top: 15.2%;
        left: 52.8%;
        width: 70%;
        transform: translateX(-50%);
    }
  }

  &__photo--second {
    top: 12%;
    right: 11.2%;
    width: 50.6%;

    @media (max-width: $mobile) {
      top: 10.8%;
      right: auto;
      left: 42.7%;
      width: 73%;
      transform: translateX(-50%);
    }
  }

  &__frame-piece {
    z-index: 3;
  }

  &__frame-piece--first {
    top: 19.7rem;
    left: 0;
    width: 18.7rem;

    @media (max-width: $tablet) {
      top: 15.8rem;
      width: 15rem;
    }

    @media (max-width: $mobile) {
      top: 32.2%;
      left: 0;
      width: 30.6%;
    }
  }

  &__frame-piece--second {
    top: 14.4rem;
    right: 0;
    width: 16.5rem;

    @media (max-width: $tablet) {
      top: 11.7rem;
      width: 13.4rem;
    }

    @media (max-width: $mobile) {
      top: 24.75%;
      right: 0;
      width: 28.35%;
    }
  }

  &__info {
    position: absolute;
    z-index: 4;
    display: flex;
    flex-direction: column;
    justify-content: center;
    min-height: 15.2rem;
    padding: 2.8rem;
    border-radius: 30px;
    background-color: $white;
    color: $black;

    @media (max-width: $tablet) {
      min-height: 12.2rem;
      padding: 1.8rem 2.4rem;
    }

    @media (max-width: $mobile) {
      right: 0;
      bottom: 0;
      left: 0;
      width: 100%;
      min-height: 0;
      padding: 1.4rem 1.6rem;
    }
  }

  &__info--first {
    top: 10.8%;
    left: 51.9%;
    width: 48.2%;

    @media (max-width: $mobile) {
      top: auto;
      left: 0;
      width: 70%;
    }
  }

  &__info--second {
    top: 56.5%;
    left: 0;
    width: 54.2%;

    @media (max-width: $mobile) {
      top: auto;
      right: 0;
      bottom: 1.6rem;
      left: auto;
      width: 70%;
    }
  }

  &__info--green {
    --events-title-color: #{$green};
  }

  &__info--turquoise {
    --events-title-color: #{$turquoise};
  }

  &__item-title {
    margin: 0;
    color: var(--events-title-color);
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(3.2rem, 1.1, 900);

    @media (max-width: $tablet) {
      font-size: 2.8rem;
    }

    @media (max-width: $mobile) {
      font-size: 2.2rem;
    }

    @media (max-width: $mobile-s) {
      font-size: 2rem;
    }
  }

  &__item-title-line {
    display: block;
  }

  &__description {
    margin: 0.6rem 0 0;
    font-style: normal;
    @include font(2rem, 1.1, 700);

    @media (max-width: $tablet) {
      font-size: 1.8rem;
    }

    @media (max-width: $mobile) {
      margin-top: 0.4rem;
      font-size: 1.4rem;
    }
  }
}
</style>
