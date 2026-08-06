<script setup>
import { sanitizeText } from '~/utils/sanitize'

defineProps({
  item: { type: Object, required: true },
  tabletVariant: { type: String, default: '' },
})
</script>

<template>
  <article
    class="zone-card"
    :class="[
      `zone-card--${item.variant}`,
      `zone-card--${item.id}`,
      tabletVariant && `zone-card--tablet-${tabletVariant}`,
    ]"
  >
    <p
      v-if="item.age"
      class="zone-card__age"
      v-html="sanitizeText(item.age)"
    />

    <div class="zone-card__content">
      <h3 class="zone-card__title" v-html="sanitizeText(item.title)" />
      <p
        class="zone-card__description"
        v-html="sanitizeText(item.description)"
      />
      <p class="zone-card__time" v-html="sanitizeText(item.time)" />
    </div>

    <Button
      v-if="item.button?.text"
      class="zone-card__button"
      :to="item.button.link"
      variant="registration"
    >
      <span v-html="sanitizeText(item.button.text)" />
    </Button>

    <Image
      v-if="item.image"
      class="zone-card__decor"
      :src="item.image"
      alt=""
      :width="item.imageWidth"
      :height="item.imageHeight"
      :img-attrs="{ 'aria-hidden': 'true' }"
    />
  </article>
</template>

<style lang="scss">
.zone-card {
  position: relative;
  min-height: 28rem;
  height: 100%;
  padding: 2.6rem 2.4rem;
  overflow: hidden;
  border-radius: 20px;
  color: $white;
  display: flex;
  flex-direction: column;

  @media (max-width: $mobile) {
    height: 28rem;
    min-height: 0;
    padding: 2.2rem 2rem;
  }

  &--green {
    background-color: $green;
  }

  &--turquoise {
    background-color: $turquoise;
  }

  &--tablet-green {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      background-color: $green;
    }
  }

  &--tablet-turquoise {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      background-color: $turquoise;
    }
  }

  &__content {
    position: relative;
    z-index: 2;
  }

  &__age {
    position: absolute;
    top: 1.6rem;
    right: 2rem;
    z-index: 3;
    margin: 0;
    color: $white;
    text-align: right;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(1.6rem, 1.1, 900);
  }

  &__title {
    margin: 0;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(2.8rem, 1, 900);

    @media (max-width: $mobile) {
      max-width: 23rem;
      font-size: 2.5rem;
    }
  }

  &__description {
    position: relative;
    z-index: 2;
    margin: 1.3rem 0 0;
    font-style: normal;
    @include font(2rem, 1.1, 700);

    @media (max-width: $tablet) {
      font-size: 1.8rem;
    }

    @media (max-width: $mobile) {
      max-width: 23rem;
      margin-top: 0.8rem;
      font-size: 1.6rem;
    }
  }

  &__time {
    position: relative;
    z-index: 2;
    margin: 1.2rem 0 1rem;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(1.6rem, 1.1, 700);

    @media (max-width: $mobile) {
      margin-top: 1.2rem;
      font-size: 1.5rem;
    }
  }

  &__button {
   max-width: fit-content;
    margin-top: auto;
    position: relative;
    z-index: 3;
  }

  &__decor {
    position: absolute;
    right: 0;
    bottom: 0;
    z-index: 1;
    width: 19rem;
    max-width: none;
    height: auto;
    mix-blend-mode: hard-light;
    pointer-events: none;
    user-select: none;

     @media (max-width: $tablet) {
      width: 17rem;
    }

    @media (max-width: $mobile) {
      width: 18rem;
    }
  }


  &--golf .zone-card__decor {
    width: 21rem;

    @media (max-width: $mobile) {
      width: 18rem;
    }
  }

  &--tennis .zone-card__decor {
    @media (max-width: $mobile) {
      width: 18rem;
    }
  }

  &--volleyball .zone-card__decor {
    @media (max-width: $mobile) {
      width: 17.5rem;
    }
  }

  &--begovel .zone-card__decor {
   width: 24rem;

    @media (max-width: $mobile) {
      width: 20rem;
    }
  }

  &--jumping .zone-card__decor {
      width: 22rem;

    @media (max-width: $mobile) {
      width: 17rem;
    }
  }
}
</style>
