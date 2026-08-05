<script setup>
import { sanitizeText } from '~/utils/sanitize'

defineProps({
  item: { type: Object, required: true },
  tabletVariant: { type: String, default: '' },
})
</script>

<template>
  <article
    class="activity-card"
    :class="[
      `activity-card--${item.variant}`,
      tabletVariant && `activity-card--tablet-${tabletVariant}`,
    ]"
  >
    <div class="activity-card__content">
      <h3 class="activity-card__title" v-html="sanitizeText(item.title)" />
      <p
        class="activity-card__description"
        v-html="sanitizeText(item.description)"
      />
      <p class="activity-card__time" v-html="sanitizeText(item.time)" />
    </div>

    <Button
      v-if="item.button?.text"
      class="activity-card__button"
      :to="item.button.link"
      :variant="item.variant === 'white'
        ? 'registration-cyan'
        : 'registration'"
    >
      <span v-html="sanitizeText(item.button.text)" />
    </Button>
  </article>
</template>

<style lang="scss">
.activity-card {
  display: flex;
  flex-direction: column;
  min-height: 26rem;
  padding: 2.4rem;
  border-radius: 20px;

  @media (max-width: $mobile) {
    height: 24rem;
    min-height: 0;
    padding: 2.2rem 2rem;
  }

  &--cyan {
    background-color: $cyan;
    color: $white;
  }

  &--white {
    background-color: $white;
    color: $black;
  }

  &--tablet-cyan {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      background-color: $cyan;
      color: $white;
    }
  }

  &--tablet-white {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      background-color: $white;
      color: $black;
    }
  }

  &__content {
    display: flex;
    flex-direction: column;
  }

  &__title {
    margin: 0;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(2.8rem, 1, 900);

    @media (max-width: $tablet) {
      font-size: 2.6rem;
    }

    @media (max-width: $mobile) {
      font-size: 2.4rem;
    }
  }

  &--white .activity-card__title {
    color: $cyan;
  }

  &--tablet-cyan .activity-card__title {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      color: $white;
    }
  }

  &--tablet-white .activity-card__title {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      color: $cyan;
    }
  }

  &__description {
    margin: 1rem 0 0;
    font-style: normal;
    @include font(2rem, 1.1, 700);

    @media (max-width: $mobile) {
      margin-top: 0.8rem;
      font-size: 1.6rem;
    }
  }

  &__time {
    margin: 1.2rem 0 1rem;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(1.6rem, 1, 700);

    @media (max-width: $mobile) {
      font-size: 1.5rem;
    }
  }

  &--white .activity-card__time {
    color: $cyan;
  }

  &--tablet-cyan .activity-card__time {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      color: $white;
    }
  }

  &--tablet-white .activity-card__time {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      color: $cyan;
    }
  }

  &__button {
    position: relative;
    z-index: 2;
    max-width: fit-content;
    margin-top: auto;
  }

  &--tablet-cyan .activity-card__button {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      background-color: $white;
      color: $cyan;
    }
  }

  &--tablet-white .activity-card__button {
    @media (min-width: calc($mobile + 1px)) and (max-width: $tablet) {
      background-color: $cyan-secondary;
      color: $white;
    }
  }
}
</style>
