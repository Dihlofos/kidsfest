<script setup>
import {
  computed,
  nextTick,
  onBeforeUnmount,
  onMounted,
  ref,
} from 'vue'
import { sanitizeText } from '~/utils/sanitize'

const props = defineProps({
  title: { type: String, required: true },
  description: { type: String, default: '' },
  locations: { type: Array, default: () => [] },
  markers: { type: Array, default: () => [] },
  images: { type: Object, default: () => ({}) },
})

const activeMarkerId = ref(null)
const activeLocationId = ref(null)
const listItemRefs = ref([])
const listRef = ref(null)
const mapRef = ref(null)
const mapListHeight = ref(null)
let mapResizeObserver = null

function shouldUseStackedLayout() {
  return (
    typeof window !== 'undefined' &&
    window.matchMedia('(max-width: 1024px)').matches
  )
}

function updateMapListHeight() {
  if (typeof window === 'undefined' || !mapRef.value) return

  if (shouldUseStackedLayout()) {
    mapListHeight.value = null
    return
  }

  mapListHeight.value = mapRef.value.getBoundingClientRect().height
}

function scrollElementToViewportCenter(element) {
  if (typeof window === 'undefined' || !element) return

  const rect = element.getBoundingClientRect()
  const nextScrollTop =
    window.scrollY + rect.top + rect.height / 2 - window.innerHeight / 2

  window.scrollTo({
    top: Math.max(0, nextScrollTop),
    behavior: 'smooth',
  })
}

function scrollListToLocation(
  locationId,
  { align = 'center', behavior = 'smooth' } = {},
) {
  const index = props.locations.findIndex(
    (location) => location.id === locationId,
  )
  const element = listItemRefs.value[index]
  const list = listRef.value

  if (!element || !list) return

  const elementTop =
    element.getBoundingClientRect().top -
    list.getBoundingClientRect().top +
    list.scrollTop
  const maxScrollTop = Math.max(0, list.scrollHeight - list.clientHeight)
  const targetScrollTop =
    align === 'start'
      ? elementTop
      : elementTop - list.clientHeight / 2 + element.clientHeight / 2
  const nextScrollTop = Math.min(Math.max(0, targetScrollTop), maxScrollTop)

  if (behavior === 'auto') {
    list.scrollTop = nextScrollTop
    return
  }

  list.scrollTo({
    top: nextScrollTop,
    behavior,
  })
}

function selectMarker(marker) {
  activeMarkerId.value = marker.id
  activeLocationId.value = marker.number

  nextTick(() => {
    if (shouldUseStackedLayout()) {
      scrollListToLocation(marker.number, {
        align: 'start',
        behavior: 'auto',
      })
      scrollElementToViewportCenter(listRef.value)

      requestAnimationFrame(() => {
        scrollListToLocation(marker.number, {
          align: 'start',
          behavior: 'auto',
        })
      })
      return
    }

    scrollListToLocation(marker.number)
  })
}

function selectLocation(locationId) {
  activeLocationId.value = locationId
  activeMarkerId.value =
    props.markers.find((marker) => marker.number === locationId)?.id ?? null

  nextTick(() => {
    if (shouldUseStackedLayout()) {
      scrollElementToViewportCenter(mapRef.value)
      return
    }

    scrollListToLocation(locationId)
  })
}

function getMarkerStyle(marker) {
  return {
    left: `${marker.x}%`,
    top: `${marker.y}%`,
  }
}

const mapContentStyle = computed(() => {
  if (mapListHeight.value === null) return {}

  return {
    '--map-list-height': `${mapListHeight.value}px`,
  }
})

onMounted(() => {
  window.addEventListener('resize', updateMapListHeight)
  nextTick(updateMapListHeight)

  if ('ResizeObserver' in window) {
    mapResizeObserver = new ResizeObserver(updateMapListHeight)

    if (mapRef.value) {
      mapResizeObserver.observe(mapRef.value)
    }
  }
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateMapListHeight)
  mapResizeObserver?.disconnect()
})
</script>

<template>
  <section id="map" class="map">
    <Container class="map__container">
      <h2 class="map__title" v-html="sanitizeText(title)" />

      <p
        v-if="description"
        class="map__description"
        v-html="sanitizeText(description)"
      />

      <div class="map__content" :style="mapContentStyle">
        <ul
          v-if="locations.length"
          ref="listRef"
          class="map__list"
          aria-label="Площадки фестиваля"
        >
          <li
            v-for="(location, index) in locations"
            :key="location.id"
            :ref="
              (element) => {
                if (element) listItemRefs[index] = element
              }
            "
            class="map__item"
            :class="{
              'map__item--active': location.id === activeLocationId,
            }"
          >
            <button
              class="map__item-button"
              type="button"
              :aria-pressed="location.id === activeLocationId"
              @click="selectLocation(location.id)"
            >
              <span class="map__item-number">{{ location.id }}</span>
              <span
                class="map__item-title"
                v-html="sanitizeText(location.title)"
              />
            </button>
          </li>
        </ul>

        <div v-if="images.map" class="map__viewport">
          <div
            ref="mapRef"
            class="map__canvas"
            role="group"
            aria-label="Интерактивная карта площадок фестиваля"
          >
            <Image
              class="map__image"
              :src="images.map"
              :alt="images.alt || 'Карта площадок фестиваля'"
              width="1788"
              height="1272"
            />

            <button
              v-for="marker in markers"
              :key="marker.id"
              class="map__pin"
              :class="{
                'map__pin--active': marker.id === activeMarkerId,
              }"
              :style="getMarkerStyle(marker)"
              type="button"
              :aria-label="`${marker.number}. ${marker.title}`"
              :aria-pressed="marker.id === activeMarkerId"
              @click="selectMarker(marker)"
            >
              <Image
                v-if="images.marker"
                class="map__pin-marker"
                :src="images.marker"
                alt=""
                width="102"
                height="142"
                :img-attrs="{ 'aria-hidden': 'true' }"
              />
              <span class="map__pin-number">{{ marker.number }}</span>
            </button>
          </div>
        </div>
      </div>
    </Container>
  </section>
</template>

<style lang="scss">
.map {
  position: relative;
  z-index: 4;
  padding: 7.5rem 0 11rem;
  overflow: hidden;
  background-color: $white;
  color: $black;

  @media (max-width: $tablet) {
    padding-top: 9rem;
    padding-bottom: 11rem;
  }

  @media (max-width: $mobile) {
    padding-top: 7rem;
    padding-bottom: 8rem;
  }

  &__title {
    margin: 0;
    font-family: $als-trigger-luzhniki;
    font-style: normal;
    text-transform: uppercase;
    @include font(6.8rem, 0.95, 700);

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

  &__description {
    max-width: 52rem;
    margin: 1.2rem 0 0;
    font-style: normal;
    @include font(2rem, 1.25, 700);

    @media (max-width: $mobile) {
      margin-top: 1.4rem;
      font-size: 1.7rem;
    }
  }

  &__content {
    display: grid;
    grid-template-columns: minmax(42rem, 70rem) minmax(52rem, 68.9rem);
    gap: 3.4rem;
    align-items: start;
    margin-top: 4.2rem;

    @media (max-width: $tablet) {
      grid-template-columns: 1fr;
      gap: 4.8rem;
      margin-top: 4rem;
    }

    @media (max-width: $mobile) {
      gap: 3.6rem;
      margin-top: 3.2rem;
    }
  }

  &__list {
    width: 100%;
    height: var(--map-list-height, auto);
    max-height: var(--map-list-height, 53rem);
    min-height: 0;
    margin: 0;
    padding: 0;
    list-style: none;
    overflow-y: auto;
    scrollbar-color: $green transparent;
    scrollbar-width: thin;

    @media (max-width: $tablet) {
      height: auto;
      max-height: 32rem;
    }

    @media (max-width: $mobile) {
      max-height: 30rem;
    }
  }

  &__item {
    border-bottom: 0.1rem solid rgba($black, 0.08);
    transition: background-color 180ms ease;

    @media (hover: hover) {
      &:hover {
        background-color: rgba($green, 0.06);
      }
    }
  }

  &__item--active {
    background-color: rgba($green, 0.12);

    @media (hover: hover) {
      &:hover {
        background-color: rgba($green, 0.12);
      }
    }
  }

  &__item-button {
    display: grid;
    width: 100%;
    min-height: 8.2rem;
    grid-template-columns: 4.4rem minmax(0, 1fr);
    align-items: center;
    gap: 1.8rem;
    padding: 1.2rem 1.4rem;
    border: 0;
    background-color: transparent;
    color: $black;
    text-align: left;
    cursor: pointer;

    &:focus-visible {
      outline: 0.3rem solid $cyan;
      outline-offset: -0.3rem;
    }

    @media (max-width: $tablet) {
      min-height: 7.2rem;
      padding: 1rem 1.2rem;
    }

    @media (max-width: $mobile) {
      min-height: 6.6rem;
      grid-template-columns: 3.8rem minmax(0, 1fr);
      gap: 1.4rem;
    }
  }

  &__item-number {
    display: inline-flex;
    width: 4.4rem;
    height: 4.4rem;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background-color: rgba($green, 0.32);
    color: $black;
    font-family: $als-trigger-luzhniki;
    @include font(1.8rem, 1.3, 700);
    letter-spacing: -0.14px;

    @media (max-width: $mobile) {
      width: 3.8rem;
      height: 3.8rem;
      font-size: 1.6rem;
    }
  }

  &__item-title {
    min-width: 0;
    word-break: break-word;
    @include font(2rem, 1.1, 700);

    @media (max-width: $mobile) {
      font-size: 1.7rem;
    }
  }

  &__viewport {
    min-width: 0;
  }

  &__canvas {
    position: relative;
    width: 100%;
    aspect-ratio: 1788 / 1272;
    overflow: hidden;
    border-radius: 20px;
    background-color: $cyan-secondary;
  }

  &__image {
    position: absolute;
    inset: 0;
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;
    user-select: none;
  }

  &__pin {
    position: absolute;
    z-index: 2;
    display: inline-flex;
    width: 3.2rem;
    height: 3.2rem;
    padding: 0;
    align-items: center;
    justify-content: center;
    border: 0;
    background-color: transparent;
    transform: translate(-50%, -50%);
    transform-origin: center;
    cursor: pointer;
    transition: transform 250ms ease;

    &:focus-visible {
      outline: 0.3rem solid $cyan;
      outline-offset: 0.3rem;
    }

    @media (hover: hover) {
      &:hover {
        transform: translate(-50%, -50%) scale(1.08);
      }
    }

    @media (max-width: $mobile) {
      width: 2rem;
      height: 2rem;
    }
  }

  &__pin--active {
    z-index: 3;
    transform: translate(-50%, calc(-50% + 0.9rem)) scale(1.04);

    @media (hover: hover) {
      &:hover {
        transform: translate(-50%, calc(-50% + 0.9rem)) scale(1.04);
      }
    }

    @media (max-width: $mobile) {
      transform: translate(-50%, calc(-50% + 0.2rem));
    }
  }

  &__pin-marker {
    position: absolute;
    top: 50%;
    left: 50%;
    z-index: 0;
    display: block;
    width: 4.4rem;
    max-width: none;
    height: auto;
    opacity: 0;
    filter: drop-shadow(0 0.3rem 0.6rem rgba($black, 0.22));
    transform: translate(-50%, -100%) scale(0.15);
    transform-origin: 50% 100%;
    transition:
      opacity 160ms ease,
      transform 380ms cubic-bezier(0.34, 1.56, 0.64, 1);
    pointer-events: none;
    user-select: none;

    @media (max-width: $mobile) {
      width: 2.6rem;
    }
  }

  &__pin--active .map__pin-marker {
    opacity: 1;
    transform: translate(-50%, -100%) scale(1);
  }

  &__pin-number {
    position: relative;
    z-index: 1;
    display: inline-flex;
    width: 3.2rem;
    height: 3.2rem;
    align-items: center;
    justify-content: center;
    border: 1.5px solid $white;
    border-radius: 50%;
    background-color: $green;
    color: $white;
    font-family: $als-trigger-luzhniki;
    transition: transform 380ms cubic-bezier(0.34, 1.56, 0.64, 1);
    @include font(1.3rem, 1.3, 700);
    letter-spacing: -0.14px;

    @media (max-width: $mobile) {
      width: 2rem;
      height: 2rem;
      border-width: 0.15rem;
      font-size: 1rem;
    }
  }

  &__pin--active .map__pin-number {
    transform: translateY(-3.9rem) scale(1.04);

    @media (max-width: $mobile) {
      transform: translateY(-2.3rem);
    }
  }
}
</style>
