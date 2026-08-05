<script setup>
import footerData from '~/../data/sections/footer.json'
import { sanitizeText } from '~/utils/sanitize'
</script>

<template>
  <footer id="contact" class="footer">
    <Container class="footer__container">
      <Image
        v-if="footerData.images?.decor"
        class="footer__decor"
        :src="footerData.images.decor"
        alt=""
        width="2261"
        height="2261"
        :img-attrs="{ 'aria-hidden': 'true' }"
      />

      <div class="footer__wrapper">
        <div class="footer__lead">
          <div
            class="footer__title"
            v-html="sanitizeText(footerData.title)"
          />

          <Button
            v-if="footerData.registration?.text"
            class="footer__button"
            :to="footerData.registration.link"
            variant="black"
            v-anchor
          >
            {{ footerData.registration.text }}
          </Button>
        </div>

        <div class="footer__content">
          <div class="footer__contacts">
            <div
              v-for="contact in footerData.contacts"
              :key="contact.email"
              class="footer__contact"
            >
              <div
                class="footer__contact-label"
                v-html="sanitizeText(contact.label)"
              />
              <a
                :href="`mailto:${contact.email}`"
                class="footer__contact-link"
              >
                {{ contact.email }}
              </a>
            </div>
          </div>

          <div class="footer__socials" aria-label="Социальные сети">
            <a
              v-for="social in footerData.socials"
              :key="social.type"
              class="footer__social"
              :href="social.href"
              target="_blank"
              rel="noopener noreferrer"
              :aria-label="social.label"
            >
              <Image
                class="footer__social-icon"
                :src="social.src"
                alt=""
                width="32"
                height="32"
                :img-attrs="{ 'aria-hidden': 'true' }"
              />
            </a>
          </div>

          <div class="footer__docs">
            <a
              v-for="doc in footerData.docs"
              :key="doc.href"
              :href="doc.href"
              class="footer__doc"
              target="_blank"
              rel="noopener noreferrer"
              v-html="sanitizeText(doc.label)"
            ></a>
          </div>
        </div>
      </div>
    </Container>
  </footer>
</template>

<style lang="scss">
.footer {
  position: relative;
  overflow: hidden;
  min-height: 51.9rem;
  background-color: $green;
  color: $white;

  @media (max-width: $tablet) {
    min-height: initial;
    padding: 6rem 0;
  }

  @media (max-width: $mobile) {
    padding: 5rem 0;
  }

  &__container {
    position: relative;
    min-height: 51.9rem;

    @media (max-width: $tablet) {
      min-height: initial;
    }
  }

  &__wrapper {
    position: relative;
    z-index: 2;
    display: grid;
    grid-template-columns: 51rem 1fr;
    gap: 10.2rem;
    padding-top: 6.6rem;

    @media (max-width: $tablet) {
      grid-template-columns: 1fr;
      gap: 5rem;
      padding-top: 0;
    }

    @media (max-width: $mobile) {
      gap: 4rem;
    }
  }

  &__lead {
    display: flex;
    flex-direction: column;
    align-items: flex-start;

    @media (max-width: $tablet) {
      padding-top: 0;
    }
  }

  &__title {
    width: 51rem;
    margin: 0;
    color: $white;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    text-transform: uppercase;
    @include font(6.4rem, 1, 700);

    @media (max-width: $tablet) {
      width: 43rem;
      font-size: 5.2rem;
    }

    @media (max-width: $mobile) {
      width: 100%;
      max-width: 35rem;
      font-size: 4.2rem;
      letter-spacing: -0.084rem;
    }

    @media (max-width: $mobile-s) {
      font-size: 3.8rem;
    }
  }

  &__button {
    margin-top: 5rem;

    @media (max-width: $tablet) {
      margin-top: 4rem;
    }

    @media (max-width: $mobile) {
      width: 100%;
      max-width: 32.1rem;
      margin-top: 3.2rem;
    }
  }

  &__content {
    padding-top: 0.6rem;

    @media (max-width: $tablet) {
      padding-top: 0;
    }
  }

  &__contacts {
    display: grid;
    grid-template-columns: 22rem 21.7rem;
    gap: 8.7rem;

    @media (max-width: $tablet) {
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 4rem;
    }

    @media (max-width: $mobile) {
      grid-template-columns: 1fr;
      gap: 2.4rem;
    }
  }

  &__contact {
    display: flex;
    flex-direction: column;
    gap: 0.8rem;
  }

  &__contact-label {
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    white-space: nowrap;
    @include font(2.4rem, 1.2, 500);

    @media (max-width: $mobile) {
      font-size: 2.4rem;
    }
  }

  &__contact-link {
    width: fit-content;
    color: $white;
    font-family: $montserrat;
    font-style: normal;
    text-decoration: underline;
    text-decoration-thickness: 0.1rem;
    text-underline-offset: 0.2rem;
    transition: opacity 150ms ease;
    @include font(2rem, 1.2, 500);

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
  }

  &__socials {
    display: flex;
    align-items: center;
    gap: 2.4rem;
    margin-top: 8.4rem;

    @media (max-width: $tablet) {
      margin-top: 5rem;
    }

    @media (max-width: $mobile) {
      margin-top: 3.6rem;
    }
  }

  &__social {
    display: flex;
    width: 3.2rem;
    height: 3.2rem;
    align-items: center;
    justify-content: center;
    flex: 0 0 3.2rem;
    text-decoration: none;
    user-select: none;

    &:focus-visible {
      outline: 0.2rem solid $black;
      outline-offset: 0.3rem;
    }

    &:focus-visible .footer__social-icon {
      filter: brightness(0);
    }

    @media (hover: hover) {
      &:hover .footer__social-icon {
        filter: brightness(0);
      }
    }
  }

  &__social-icon {
    display: block;
    width: 3.2rem;
    height: 3.2rem;
    filter: brightness(0) invert(1);
    transition: filter 200ms ease;
  }

  &__docs {
    display: grid;
   grid-template-columns: 28rem 28rem;
    gap: 1.6rem 2.3rem;
    margin-top: 5.2rem;

    @media (max-width: $tablet) {
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 1.8rem 4rem;
      margin-top: 4rem;
    }

    @media (max-width: $mobile) {
      grid-template-columns: 1fr;
      gap: 1.6rem;
      margin-top: 3.2rem;
    }
  }

  &__doc {
    width: fit-content;
    color: $white;
    font-family: $montserrat;
    font-style: normal;
    text-decoration: underline;
    text-decoration-thickness: 0.1rem;
    text-underline-offset: 0.2rem;
    transition: opacity 150ms ease;
    @include font(1.4rem, 1.3, 500);

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
  }

  &__doc:nth-child(3) {
    grid-column: 1;

    @media (max-width: $mobile) {
      grid-column: auto;
    }
  }

  &__decor {
    position: absolute;
    z-index: 1;
    top: -38.5rem;
    left: -34.5rem;
    width: 113rem;
    max-width: none;
    pointer-events: none;
    user-select: none;

    @media (max-width: $tablet) {
       display: none;
    }
  }
}
</style>
