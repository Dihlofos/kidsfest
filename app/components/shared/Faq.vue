<script setup>
import { ref } from 'vue'
import { sanitizeText } from '~/utils/sanitize'

defineProps({
  title: {
    type: String,
    default: 'Вопросы и ответы',
  },
  items: {
    type: Array,
    default: () => [],
  },
  images: {
    type: Object,
    default: () => ({
      decorLeft: '/images/decor/faq-decor-left.png',
      decorRight: '/images/decor/faq-decor-right.png',
      arrow: '/images/arrow.svg',
    }),
  },
})

const openState = ref({})

function isOpen(index) {
  return Boolean(openState.value[index])
}

function toggle(index) {
  openState.value = {
    ...openState.value,
    [index]: !openState.value[index],
  }
}
</script>

<template>
  <section id="faq" class="faq">
    <Container class="faq__container">
      <Image
        v-if="images.decorLeft"
        class="faq__decor faq__decor--left"
        :src="images.decorLeft"
        alt=""
        width="1234"
        height="1622"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />

      <Image
        v-if="images.decorRight"
        class="faq__decor faq__decor--right"
        :src="images.decorRight"
        alt=""
        width="1234"
        height="1622"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />

      <div class="faq__wrapper">
        <h2 class="faq__title" v-html="sanitizeText(title)" />

        <div class="faq__accordion">
          <div
            v-for="(item, index) in items"
            :key="item.question"
            class="faq__item"
            :class="{ 'faq__item--active': isOpen(index) }"
            @click="toggle(index)"
          >
            <button
              class="faq__toggler"
              type="button"
              :aria-expanded="isOpen(index)"
              :aria-controls="`faq-answer-${index}`"
            >
              <span
                class="faq__question"
                v-html="sanitizeText(item.question)"
              />

              <span class="faq__icon" aria-hidden="true">
                <Image
                  class="faq__arrow"
                  :src="images.arrow"
                  alt=""
                  width="19"
                  height="11"
                  :img-attrs="{ 'aria-hidden': 'true' }"
                />
              </span>
            </button>

            <div
              :id="`faq-answer-${index}`"
              class="faq__content"
              :class="{ 'faq__content--active': isOpen(index) }"
              role="region"
              :aria-hidden="!isOpen(index)"
            >
              <div class="faq__content-inner">
                <p
                  class="faq__answer"
                  v-html="sanitizeText(item.answer)"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </Container>
  </section>
</template>

<style lang="scss">
.faq {
  position: relative;
  z-index: 2;
  overflow: hidden;
  padding: 6.6rem 0 11.8rem;
  background: $turquoise;
  color: $black;

  @media (max-width: $tablet) {
    padding: 6.4rem 0 8rem;
  }

  @media (max-width: $mobile) {
    padding: 4.8rem 0 6rem;
  }

  &__container {
    position: relative;
  }

  &__wrapper {
    position: relative;
    z-index: 2;
  }

  &__title {
    margin: 0 0 5.2rem;
    color: $white;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    text-align: center;
    text-transform: uppercase;
    @include font(6.8rem, 0.95, 700);

    @media (max-width: $tablet) {
      margin-bottom: 3.8rem;
      font-size: 5.2rem;
    }

    @media (max-width: $mobile) {
      margin-bottom: 2.8rem;
      font-size: 3.8rem;
    }

    @media (max-width: $mobile-s) {
      font-size: 3.2rem;
    }
  }

  &__accordion {
    width: 100%;
    max-width: 99.6rem;
    margin: 0 auto;
    padding: 1.6rem 4rem 0.8rem;
    border-radius: 20px;
    background: $white;

    @media (max-width: $tablet) {
      padding-inline: 2.8rem;
    }

    @media (max-width: $mobile) {
      padding: 0.8rem 1.6rem 0.6rem;
      border-radius: 16px;
    }
  }

  &__item {
    border-bottom: 0.1rem solid $cyan-secondary;
    cursor: pointer;
  }

  &__item:last-child {
    border-bottom: 0;
  }

  &__toggler {
    display: flex;
    width: 100%;
    min-height: 7.7rem;
    align-items: center;
    justify-content: space-between;
    gap: 2rem;
    padding: 0;
    border: 0;
    background: transparent;
    color: $cyan;
    font-style: normal;
    text-align: left;
    cursor: pointer;
    transition: min-height 300ms ease;
    @include font(2rem, 1.16, 700);

    @media (max-width: $mobile) {
      min-height: 6.4rem;
      gap: 1.2rem;
      font-size: 1.6rem;
    }
  }

  &__item--active &__toggler {
    min-height: 5.2rem;

    @media (max-width: $mobile) {
      min-height: 5.6rem;
    }
  }

  &__question {
    padding-block: 1rem;
  }

  &__icon {
    display: flex;
    width: 3.8rem;
    height: 3.8rem;
    flex: 0 0 3.8rem;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background: $green;
    transition: background-color 0.2s ease;

    @media (max-width: $mobile) {
      width: 3.2rem;
      height: 3.2rem;
      flex-basis: 3.2rem;
    }
  }

  &__arrow {
    display: block;
    width: 1.9rem;
    height: 1.1rem;
    padding-top: 0.3rem;
    transition: transform 0.2s ease;

    @media (max-width: $mobile) {
      width: 1.6rem;
      height: auto;
    }
  }

  &__item--active &__arrow {
    transform: rotate(180deg);
  }

  &__content {
    display: grid;
    grid-template-rows: 0fr;
    visibility: hidden;
    opacity: 0;
    transition:
      grid-template-rows 350ms ease,
      opacity 250ms ease,
      visibility 0s linear 350ms;
  }

  &__content--active {
    grid-template-rows: 1fr;
    visibility: visible;
    opacity: 1;
    transition:
      grid-template-rows 350ms ease,
      opacity 250ms ease 80ms,
      visibility 0s;
  }

  &__content-inner {
    min-height: 0;
    overflow: hidden;
  }

  &__answer {
    max-width: 80rem;
    margin: 0;
    padding: 0 0 1.4rem;
    font-style: normal;
    @include font(1.6rem, 1.3, 400);

    @media (max-width: $mobile) {
      padding-bottom: 1.6rem;
      font-size: 1.4rem;
    }
  }

  &__decor {
    position: absolute;
    z-index: 1;
    max-width: none;
    pointer-events: none;
    user-select: none;
  }

  &__decor--left {
    top: -29.5rem;
    left: -30rem;
    width: 61rem;

    @media (max-width: $tablet) {
      top: -11rem;
      left: -43rem;
      width: 58rem;
    }

    @media (max-width: $mobile) {
      top: -8rem;
      left: -32rem;
      width: 43rem;
      opacity: 0.7;
    }
  }

  &__decor--right {
    top: 42.5rem;
    right: -34rem;
    width: 64rem;

    @media (max-width: $tablet) {
      top: auto;
      right: -37rem;
      bottom: -13rem;
      width: 58rem;
    }

    @media (max-width: $mobile) {
      right: -31rem;
      bottom: -12rem;
      width: 43rem;
      opacity: 0.7;
    }
  }
}
</style>
