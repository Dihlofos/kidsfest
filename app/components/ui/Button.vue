<script setup>
const props = defineProps({
  variant: { type: String, default: 'primary' },
  size: { type: String, default: 'md' },
  disabled: { type: Boolean, default: false },
  to: { type: String, default: '' },
  type: { type: String, default: 'button' },
})
</script>

<template>
  <a
    v-if="to"
    :href="to"
    :class="['button', `button--${variant}`, `button--${size}`]"
  >
    <slot />
  </a>
  <button
    v-else
    :class="['button', `button--${variant}`, `button--${size}`]"
    :disabled="disabled"
    :type="type"
  >
    <slot />
  </button>
</template>

<style lang="scss">
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  border: 0;
  color: $white;
  font-style: normal;
  text-decoration: none;
  white-space: nowrap;
  cursor: pointer;
  transition:
    background-color 250ms ease,
    color 250ms ease;

  @media (hover: hover) {
    &:not(:disabled):hover {
      background-color: $white;
      color: $black;
    }
  }

  &:focus-visible {
    outline: 0.2rem solid $cyan;
    outline-offset: 0.3rem;
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}

.button--sm {
  padding-inline: 1.6rem;
}

.button--md {
  padding-inline: 2.4rem;
}

.button--lg {
  padding-inline: 4rem;
}

.button--primary,
.button--green {
  min-width: 28.2rem;
  height: 3.9rem;
  padding: 0 4.4rem;
  border-radius: 30px;
  background-color: $green;
  font-family: $als-trigger-luzhniki;
  @include font(1.6rem, 1, 700);
}

.button--secondary,
.button--black {
  min-width: 32.1rem;
  height: 6.4rem;
  padding: 0 4rem;
  border-radius: 40px;
  background-color: $black;
  font-family: $als-trigger-luzhniki;
  @include font(2rem, 1, 700);
}

.button--registration,
.button--registration-cyan {
  height: 3.5rem;
  padding-inline: 2rem;
  border-radius: 30px;
  @include font(2rem, 0.9, 700);
}

.button--registration {
  background-color: $white;
  color: $cyan;
}

.button--registration-cyan {
  background-color: $cyan-secondary;
  color: $white;
}

.button--ghost {
  padding: 0.8rem 2.4rem;
  border-radius: 8px;
  background-color: transparent;
  color: $black;
  @include font(1.6rem, 1, 700);
}

@media (hover: hover) {
  .button--registration:not(:disabled):hover,
  .button--registration-cyan:not(:disabled):hover {
    background-color: $black;
    color: $white;
  }
}
</style>
