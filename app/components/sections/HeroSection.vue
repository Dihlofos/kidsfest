<script setup>
const props = defineProps({
  title: { type: Array, required: true },
  partners: { type: Array, default: () => [] },
  date: { type: String, default: '' },
  location: { type: Array, default: () => [] },
  badge: { type: String, default: '' },
  age: { type: String, default: '' },
  button: { type: Object, default: () => ({}) },
  images: { type: Object, default: () => ({}) },
})
</script>

<template>
  <section id="hero" class="hero">
    <Image
      v-if="images.decorLeft"
      class="hero__decor hero__decor--left"
      :src="images.decorLeft"
      alt=""
      width="1566"
      height="2059"
      loading="eager"
      fetchpriority="high"
      :img-attrs="{ 'aria-hidden': 'true' }"
    />
    <Image
      v-if="images.decorRight"
      class="hero__decor hero__decor--right"
      :src="images.decorRight"
      alt=""
      width="1386"
      height="1681"
      loading="eager"
      fetchpriority="high"
      :img-attrs="{ 'aria-hidden': 'true' }"
    />

    <Container class="hero__container">
      <div class="hero__layout">
        <div class="hero__main">
          <ul v-if="partners.length" class="hero__partners" aria-label="Организаторы фестиваля">
            <li v-for="partner in partners" :key="partner.name" class="hero__partner">
              <a
                class="hero__partner-link"
                :href="partner.link"
                :aria-label="partner.name"
                target="_blank"
                rel="noopener noreferrer"
              >
                <Image
                  class="hero__partner-image"
                  :src="partner.image"
                  :alt="partner.name"
                  :width="partner.width"
                  :height="partner.height"
                  loading="eager"
                />
              </a>
            </li>
          </ul>

          <h1 class="hero__title">
            <span v-for="line in title" :key="line" class="hero__title-line">
              {{ line }}
            </span>
          </h1>

          <Button
            v-if="button?.text"
            class="hero__button"
            :to="button.link"
            variant="black"
          >
            {{ button.text }}
          </Button>
        </div>

        <div class="hero__details">
          <p v-if="date" class="hero__date">{{ date }}</p>
          <p v-if="location.length" class="hero__location">
            <span v-for="line in location" :key="line" class="hero__location-line">
              {{ line }}
            </span>
          </p>
          <p v-if="badge" class="hero__badge">{{ badge }}</p>
          <p v-if="age" class="hero__age">{{ age }}</p>
        </div>
      </div>
    </Container>
  </section>
</template>

<style lang="scss">
.hero {
  position: relative;
  z-index: 3;
  min-height: 73.4rem;
  overflow: visible;
  isolation: isolate;
  background: $main-gradient;
  color: $white;

  &::before {
    content: '';
    position: absolute;
    inset: 0;
    z-index: 0;
    background-color: rgba($black, 0.1);
    pointer-events: none;
  }

  &::after {
    content: '';
    position: absolute;
    top: calc(100% - 0.1rem);
    right: 0;
    left: 0;
    z-index: -2;
    height: 17.6rem;
    background: $main-gradient;
  }

  &__decor {
    position: absolute;
    z-index: -1;
    max-width: none;
    pointer-events: none;
    user-select: none;
  }

  &__decor--left {
    top: -4.4rem;
    left: -23rem;
    width: 78.4rem;
    height: auto;

    @media (max-width: $tablet) {
      top: -3rem;
      left: -18rem;
      width: 42rem;
      opacity: 0.5;
    }
  }

  &__decor--right {
    top: -4.6rem;
    right: -8.5rem;
    width: 69.3rem;
    height: auto;

    @media (max-width: $tablet) {
      top: 20rem;
      right: -25rem;
      width: 60rem;
      opacity: 0.7;
    }

    @media (max-width: $mobile) {
      display: none;
    }
  }

  &__container {
    position: relative;
    z-index: 1;
    min-height: inherit;
  }

  &__layout {
    display: grid;
    grid-template-columns: minmax(0, 1fr) 28.2rem;
    column-gap: 4rem;
    min-height: inherit;
    padding-top: 14rem;
    padding-bottom: 5.7rem;

    @media (max-width: $tablet) {
      grid-template-columns: minmax(0, 1fr);
      row-gap: 4rem;
      padding-top: 8rem;
      padding-bottom: 5rem;
    }
  }

  &__partners {
    display: flex;
    align-items: center;
    gap: 5.8rem;
    min-height: 3.8rem;
    margin: 0;
    padding: 0;
    list-style: none;

    @media (max-width: $tablet) {
      flex-wrap: wrap;
      gap: 2.4rem;
    }

    @media (max-width: $mobile) {
      gap: 1.6rem 2rem;
    }
  }

  &__partner-link {
    display: block;
    transition: opacity 150ms ease;
  }


  &__partner-image {
    width: auto;
    height: 3.8rem;

    @media (max-width: $mobile) {
      height: 2.8rem;
    }

   @media (max-width: $mobile-s) {
      height: 2.3rem;
    }
  }

  &__title {
    max-width: 80rem;
    margin: 4rem 0 0;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    letter-spacing: -0.208rem;
    text-transform: uppercase;
    @include font(10.4rem, 0.95, 700);

    @media (max-width: $tablet) {
      max-width: 62rem;
      letter-spacing: -0.12rem;
      font-size: 6.4rem;
    }

    @media (max-width: $mobile) {
      margin-top: 3.2rem;
      letter-spacing: -0.08rem;
      font-size: 4.6rem;
    }

      @media (max-width: $mobile-s) {
      font-size: 4.2rem;
    }
  }

  &__title-line,
  &__location-line {
    display: block;
  }

  &__button {
    margin-top: 4.2rem;

    @media (max-width: $mobile) {
      display: flex;
      width: 30rem;
      max-width: 100%;
      min-width: 0;
      height: 5.2rem;
      margin-top: 3.2rem;
      padding: 0 2.4rem;
      font-size: 1.7rem;
    }
  }

  &__details {
    padding-top: 9rem;

    @media (max-width: $tablet) {
      display: grid;
      grid-template-columns: minmax(0, 1fr) auto;
      gap: 2.4rem;
      padding-top: 0;
    }

    @media (max-width: $mobile) {
      display: grid;
      grid-template-columns: minmax(0, 1fr) auto;
      column-gap: 1.6rem;
      row-gap: 0;
    }
  }

  &__date {
    margin: 0;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    text-transform: lowercase;
    @include font(4.4rem, 1, 900);

    @media (max-width: $mobile) {
      grid-column: 1 / -1;
      font-size: 3.4rem;
    }
  }

  &__location {
    margin: 0.5rem 0 0;
    font-style: normal;
    @include font(2rem, 1.25, 700);

    @media (max-width: $mobile) {
      grid-column: 1 / -1;
      margin-top: 0.6rem;
      font-size: 1.6rem;
    }
  }

  &__badge {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 238px;
    height: 6rem;
    margin: 11.2rem 0 0;
    border-radius: 40px;
    background-color: $white;
    color: $green;
      font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(2rem, 1, 700);

    @media (max-width: $tablet) {
      grid-column: 2;
      grid-row: 1;
      margin: 0;
    }

    @media (max-width: $mobile) {
      grid-column: 1;
      grid-row: 3;
      width: 100%;
      height: 5.2rem;
      margin-top: 2.4rem;
      font-size: 1.8rem;
    }
  }

  &__age {
    margin: 7rem 0 0;
    text-align: right;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    @include font(2.8rem, 1, 900);

    @media (max-width: $tablet) {
      grid-column: 2;
      grid-row: 2;
      margin: 0;
      align-self: start;
    }

    @media (max-width: $mobile) {
      grid-column: 2;
      grid-row: 3;
      align-self: center;
      margin: 2.4rem 0 0;
      font-size: 2.4rem;
    }
  }
}
</style>
