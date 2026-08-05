<script setup>
import { navItems, registrationButton } from '~/../data/navigation.json'

const mobileOpen = ref(false)

function toggleMobile() {
  mobileOpen.value = !mobileOpen.value
}

function closeMobile() {
  mobileOpen.value = false
}
</script>

<template>
  <nav class="nav" :class="{ 'nav--open': mobileOpen }">
    <button
      class="nav__toggle"
      :class="{ 'nav__toggle--open': mobileOpen }"
      type="button"
      :aria-label="mobileOpen ? 'Закрыть меню' : 'Открыть меню'"
      :aria-expanded="mobileOpen"
      aria-controls="navigation-menu"
      @click="toggleMobile"
    >
      <span class="nav__toggle-line nav__toggle-line--top" />
      <span class="nav__toggle-line nav__toggle-line--middle" />
      <span class="nav__toggle-line nav__toggle-line--bottom" />
    </button>

    <div id="navigation-menu" class="nav__overlay">
      <ul class="nav__list">
        <li v-for="item in navItems" :key="item.link" class="nav__item">
          <a :href="item.link" class="nav__link" v-anchor @click="closeMobile">
            {{ item.label }}
          </a>
        </li>
      </ul>

      <Button
        v-if="registrationButton?.text"
        class="nav__button"
        :to="registrationButton.link"
        variant="green"
        v-anchor
        @click="closeMobile"
      >
        {{ registrationButton.text }}
      </Button>
    </div>
  </nav>
</template>

<style lang="scss">
.nav {
  width: 100%;

  &__toggle {
    display: none;
    position: relative;
    width: 6.4rem;
    height: 4.8rem;
    padding: 0;
    border: none;
    border-radius: 40px;
    background-color: $black;
    line-height: 0;
    cursor: pointer;
    transition: opacity 150ms ease;

    &:focus-visible {
      outline: 0.2rem solid $green;
      outline-offset: 0.3rem;
    }

    @media (hover: hover) {
      &:hover {
        opacity: 0.65;
      }
    }

    @media (max-width: $tablet) {
      position: fixed;
      top: 1.2rem;
      right: 2.4rem;
      z-index: 1030;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    @media (max-width: $mobile) {
      top: 1rem;
      right: 2rem;
      width: 5.2rem;
      height: 4rem;
    }
  }

  &__toggle-line {
    position: absolute;
    left: 50%;
    width: 2.6rem;
    height: 0.3rem;
    border-radius: 3px;
    background-color: $white;
    transform: translateX(-50%);
    transition:
      opacity 200ms ease,
      transform 250ms ease;

    @media (max-width: $mobile) {
      width: 2.2rem;
      height: 0.2rem;
    }
  }

  &__toggle-line--top {
    transform: translate(-50%, -0.8rem);

    @media (max-width: $mobile) {
      transform: translate(-50%, -0.6rem);
    }
  }

  &__toggle-line--bottom {
    transform: translate(-50%, 0.8rem);

    @media (max-width: $mobile) {
      transform: translate(-50%, 0.6rem);
    }
  }

  &__toggle--open &__toggle-line--top {
    transform: translateX(-50%) rotate(45deg);
  }

  &__toggle--open &__toggle-line--middle {
    opacity: 0;
  }

  &__toggle--open &__toggle-line--bottom {
    transform: translateX(-50%) rotate(-45deg);
  }

  &__overlay {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 3rem;

    @media (max-width: $tablet) {
      flex-direction: column;
      align-items: flex-start;
      justify-content: flex-start;
      position: fixed;
      inset: 0;
      z-index: 1029;
      padding: 10rem 4.5rem 4rem;
      overflow-y: auto;
      background: $main-gradient;
      opacity: 0;
      pointer-events: none;
      transition: opacity 300ms ease;
    }

    @media (max-width: $mobile) {
      padding: 9rem 2rem 3rem;
    }

    .nav--open & {
      @media (max-width: $tablet) {
        opacity: 1;
        pointer-events: all;
      }
    }
  }

  &__list {
    display: flex;
    gap: 1.6rem;
    align-items: center;
    list-style: none;
    margin: 0;
    padding: 0;

    @media (max-width: $tablet) {
      flex-direction: column;
      gap: 3.2rem;
      flex-shrink: 0;
      align-items: flex-start;
    }
  }

  &__link {
    @include font(1.6rem, 1, 700);
    color: $white;
    text-decoration: none;
    transition: opacity 150ms ease;

    &:focus-visible {
      opacity: 0.65;
      outline: 0.2rem solid currentColor;
      outline-offset: 0.3rem;
    }

    @media (hover: hover) {
      &:hover {
        opacity: 0.65;
      }
    }

    @media (max-width: $tablet) {
      font-family: $als-trigger-luzhniki;
      font-size: 3.6rem;
      padding: 0.8rem 0;
    }

    @media (max-width: $mobile) {
      font-size: 3rem;
    }
  }

  &__button.button--green {
    @media (max-width: $tablet) {
      display: flex;
      margin-top: 3.2rem;
      background-color: $black;
      color: $white;
    }

    @media (max-width: $mobile) {
      width: 100%;
      max-width: 32.1rem;
    }
  }
}
</style>
