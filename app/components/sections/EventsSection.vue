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
        class="events__decor events__decor--left"
        :src="images.decor"
        alt=""
        width="1234"
        height="1622"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />

      <Image
        v-if="images.decor"
        class="events__decor events__decor--right"
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
                :width="item.photoWidth"
                :height="item.photoHeight"
              />
            </div>

            <Image
              v-for="piece in item.framePieces"
              :key="piece.src"
              class="events__frame-piece"
              :class="`events__frame-piece--${item.id}-${piece.position}`"
              :src="piece.src"
              alt=""
              :width="piece.width"
              :height="piece.height"
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
            <Image
              v-if="item.infoDecor"
              class="events__info-decor"
              :class="`events__info-decor--${item.id}`"
              :src="item.infoDecor"
              alt=""
              :width="item.infoDecorWidth"
              :height="item.infoDecorHeight"
              :img-attrs="{ 'aria-hidden': 'true' }"
            />

            <div class="events__info-content">
              <h3 class="events__item-title">
                <span
                  v-for="line in item.title"
                  :key="line"
                  class="events__item-title-line"
                  v-html="sanitizeText(line)"
                />
              </h3>

              <p
                v-if="item.subtitle"
                class="events__subtitle"
                v-html="sanitizeText(item.subtitle)"
              />

              <div
                v-if="item.schedule?.length"
                class="events__schedule"
              >
                <div
                  v-for="entry in item.schedule"
                  :key="`${entry.label}-${entry.time}`"
                  class="events__schedule-item"
                >
                  <p
                    class="events__schedule-label"
                    v-html="sanitizeText(entry.label)"
                  />
                  <p
                    class="events__schedule-time"
                    v-html="sanitizeText(entry.time)"
                  />
                </div>
              </div>
            </div>

            <p
              v-if="item.location"
              class="events__location"
              v-html="sanitizeText(item.location)"
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
    padding-bottom: 8rem;
    border-radius: 50% 50% 0 0 / 3.6rem 3.6rem 0 0;
  }

  &__decor {
    position: absolute;
    z-index: 0;
    max-width: none;
    height: auto;
    pointer-events: none;
    user-select: none;

    @media (max-width: $tablet) {
      display: none;
    }
  }

  &__decor--left {
   top: 2.1rem;
    left: -77.4rem;
    width: 84rem;
    transform: rotate(113deg);
  }

  &__decor--right {
    right: -31rem;
    bottom: -37rem;
    width: 70rem;
    transform: rotate(171deg) scaleY(-1);

    @media (max-width: $laptop) {
      right: -22rem;
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
    height: 220rem;
    margin-top: 3rem;

    @media (max-width: $tablet) {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      height: auto;
      margin-top: 3rem;
    }

    @media (max-width: $mobile) {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 2rem;
      height: auto;
      margin-top: 3rem;
    }
  }

  &__item {
    position: absolute;
    right: 0;
    left: 0;
    width: 100%;
    height: 55rem;

    @media (max-width: $tablet) {
      position: relative;
      flex: 0 0 54rem;
      height: 54rem;
    }

    @media (max-width: $mobile) {
      position: relative;
      flex: 0 0 auto;
      width: 35.5rem;
      max-width: 100%;
      height: 43rem;
      margin: 0 auto;
    }

    @media (max-width: $xs) {
      height: 40rem;
    }
  }

  &__item--first {
    top: 0;

    @media (max-width: $mobile) {
      top: auto;
    }
  }

  &__item--second {
    top: 28rem;

    @media (max-width: $tablet) {
      top: auto;
    }

    @media (max-width: $mobile) {
      top: auto;
    }
  }

  &__item--third {
    top: 74.6rem;

    @media (max-width: $tablet) {
      top: auto;
    }

    @media (max-width: $mobile) {
      top: auto;
    }
  }

  &__item--fourth {
    top: 115.2rem;

    @media (max-width: $tablet) {
      top: auto;
    }

    @media (max-width: $mobile) {
      top: auto;
    }
  }

  &__item--fifth {
    top: 159.8rem;

    @media (max-width: $tablet) {
      top: auto;
    }

    @media (max-width: $mobile) {
      top: auto;
    }
  }

  &__visual {
    position: absolute;
    z-index: 1;
    width: 48.6rem;
    aspect-ratio: 1;

    @media (max-width: $tablet) {
      width: 41rem;
    }

    @media (max-width: $mobile) {
      top: 0;
      left: 50%;
      width: 35.5rem;
      transform: translateX(-50%);
    }
  }

  &__visual--first {
    top: 0;
    left: 0;

    @media (max-width: $mobile) {
      left: 50%;
    }
  }

  &__visual--second {
    top: 0;
    right: 0;
    width: 48rem;

    @media (max-width: $tablet) {
      right: -1rem;
      width: 41rem;
    }

    @media (max-width: $mobile) {
      right: auto;
      width: 35.5rem;
    }
  }

  &__visual--third {
    top: 0;
    left: 0;
    width: 49.3rem;

    @media (max-width: $tablet) {
      width: 39.5rem;
    }

    @media (max-width: $mobile) {
      left: 50%;
      width: 35.5rem;
    }
  }

  &__visual--fourth {
    top: 0;
    right: 2rem;
    width: 55.7rem;

    @media (max-width: $tablet) {
      right: -1rem;
      width: 44.5rem;
    }

    @media (max-width: $mobile) {
      right: auto;
      width: 35.5rem;
    }
  }

  &__visual--fifth {
    top: 0;
    left: 0;
    width: 48.6rem;

    @media (max-width: $tablet) {
      width: 39rem;
    }

    @media (max-width: $mobile) {
      left: 50%;
      width: 35.5rem;
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
    top: 0;
    left: 0;
    z-index: 1;
    width: 100%;
  }

  &__photo {
    z-index: 2;
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
    top: 10.9%;
    left: 20%;
    width: 69.45%;
  }

  &__photo--second {
    top: 11.5%;
    left: 10.8%;
    width: 73.02%;
  }

  &__photo--third {
    top: 16.8%;
    left: 16.6%;
    width: 71.4%;
  }

  &__photo--fourth {
    top: 16.2%;
    left: 16.3%;
    width: 63.4%;
  }

  &__photo--fifth {
    top: 15.7%;
    left: 17.5%;
    width: 72.02%;
  }

  &__frame-piece {
    z-index: 3;
  }

  &__frame-piece--first-main {
    top: 31.97%;
    left: 0;
    width: 30.56%;
  }

  &__frame-piece--second-main {
    top: 24.74%;
    right: 0;
    width: 28.35%;
  }

  &__frame-piece--third-top {
    bottom: 0.6%;
    left: 0;
    width: 41.38%;
  }

  &__frame-piece--third-bottom {
    top: 25.35%;
    right: 0;
    width: 41.18%;
  }

  &__frame-piece--fourth-main {
    top: 33.5%;
    left: 65%;
    width: 30.2%;
  }

  &__frame-piece--fifth-main {
    top: 28.81%;
    left: 0;
    width: 30.25%;
  }

  &__info {
    position: absolute;
    z-index: 4;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    min-height: 15rem;
    padding: 2.4rem;
    overflow: hidden;
    border-radius: 30px;
    background-color: $white;
    color: $black;

    @media (max-width: $tablet) {
      min-height: 12rem;
      padding: 1.8rem 2rem;
    }

    @media (max-width: $mobile) {
      bottom: 0;
      min-height: 0;
      padding: 1.4rem 1.6rem;
      border-radius: 24px;
    }
  }

  &__info--first {
    top: 4.8rem;
    left: 48.6rem;
    width: 44rem;
    min-height: 21.2rem;

    @media (max-width: $tablet) {
      top: 34rem;
      left: 0;
      width: 35.5rem;
      min-height: 17rem;
    }

    @media (max-width: $mobile) {
      top: auto;
      left: 0;
      width: 78%;
      min-height: 15.5rem;
    }
  }

  &__info--second {
    top: 23.4rem;
    left: 30.6rem;
    width: 44rem;

    @media (max-width: $tablet) {
      top: 36rem;
      right: 0;
      left: auto;
      width: 35.5rem;
    }

    @media (max-width: $mobile) {
      top: auto;
      right: 0;
      left: auto;
      width: 78%;
    }
  }

  &__info--third {
    top: 12.5rem;
    left: 48.6rem;
    width: 51rem;
    min-height: 18.6rem;

    @media (max-width: $tablet) {
      top: 36rem;
      left: 0;
      width: 40.8rem;
      min-height: 14.4rem;
    }

    @media (max-width: $mobile) {
      top: auto;
      right: auto;
      left: 0;
      width: 78%;
      min-height: 0;
    }
  }

  &__info--fourth {
    top: 15.5rem;
    left: 18rem;
    width: 51rem;

    @media (max-width: $tablet) {
      top: 37rem;
      right: 0;
      left: auto;
      width: 40.8rem;
    }

    @media (max-width: $mobile) {
      top: auto;
      right: 0;
      left: auto;
      width: 78%;
    }
  }

  &__info--fifth {
    top: 18rem;
    left: 48.6rem;
    width: 51rem;

    @media (max-width: $tablet) {
      top: 35.5rem;
      left: 0;
      width: 40.8rem;
    }

    @media (max-width: $mobile) {
      top: auto;
      right: auto;
      left: 0;
      width: 78%;
    }
  }

  &__info-content {
    position: relative;
    z-index: 2;
    max-width: 100%;

    @media (max-width: $mobile) {
      max-width: 72%;
    }
  }

  &__info-decor {
    position: absolute;
    right: 0;
    z-index: 1;
    max-width: none;
    height: auto;
    pointer-events: none;
    user-select: none;
  }

  &__info-decor--first {
    top: 0;
    width: 15.6rem;

    @media (max-width: $mobile) {
      width: 9rem;
    }
  }

  &__info-decor--second {
    top: 0;
    width: 15.4rem;

    @media (max-width: $mobile) {
      width: 8.5rem;
    }
  }

  &__info-decor--third {
    right: -0.1rem;
    bottom: 0;
    width: 16.6rem;

    @media (max-width: $mobile) {
      width: 9rem;
    }
  }

  &__info-decor--fourth {
    right: -0.1rem;
    bottom: 0;
    width: 16.4rem;

    @media (max-width: $mobile) {
      width: 8.5rem;
    }
  }

  &__info-decor--fifth {
    right: 0;
    bottom: 0;
    width: 18rem;

    @media (max-width: $mobile) {
      width: 9rem;
    }
  }

  &__item-title {
    margin: 0;
    color: $green;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(3.4rem, 1, 900);

    @media (max-width: $tablet) {
      font-size: 2.7rem;
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

  &__subtitle {
    margin: 0.8rem 0 0;
    color: $green;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(2.2rem, 1, 700);

    @media (max-width: $tablet) {
      font-size: 1.8rem;
    }

    @media (max-width: $mobile) {
      margin-top: 0.4rem;
      font-size: 1.6rem;
    }
  }

  &__schedule {
    position: relative;
    z-index: 2;
    margin-top: 1.4rem;
  }

  &__schedule-item {
    margin-top: 1.4rem;
  }

  &__schedule-item:first-child {
    margin-top: 0;
  }

  &__schedule-label,
  &__schedule-time,
  &__location {
    color: $cyan;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
  }

  &__schedule-label {
    margin: 0;
    @include font(2rem, 1, 700);

    @media (max-width: $tablet) {
      font-size: 1.7rem;
    }

    @media (max-width: $mobile) {
      font-size: 1.5rem;
    }
  }

  &__schedule-time {
    margin: 1rem 0 0;
    @include font(1.6rem, 1, 700);

    @media (max-width: $tablet) {
      font-size: 1.4rem;
    }

    @media (max-width: $mobile) {
      margin-top: 0.3rem;
      font-size: 1.3rem;
    }
  }

  &__location {
    position: absolute;
    right: 2.4rem;
    bottom: 2.8rem;
    z-index: 2;
    margin: 0;
    @include font(1.6rem, 1, 700);

    @media (max-width: $tablet) {
      right: 2rem;
      bottom: 2rem;
      font-size: 1.4rem;
    }

    @media (max-width: $mobile) {
      right: 1.6rem;
      bottom: 1.4rem;
      font-size: 1.2rem;
    }
  }
}
</style>
