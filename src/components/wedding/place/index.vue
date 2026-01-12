<template>
  <div
    ref="container"
    class="row items-center justify-start bg-image"
    :class="{ 'animate': isVisible }"
  >
    <div class="text-white" :class="{ 'text-center': isMobile }">
      <div
        class="font-cormorant-sc q-mt-md text-block"
        :class="textSizeClass"
        :style="isMobile ? mobileTextStyle : locationStyle"
      >
        НАША СВАДЬБА ПРОЙДЕТ В КАЗАХСТАНЕ, Г. ШУЧИНСК
      </div>

      <div
        class="font-cormorant-sc q-mt-md text-block"
        :class="textSizeClass"
        :style="isMobile ? mobileTextStyle : banquetStyle"
      >
        БАНКЕТ БУДЕТ ПРОХОДИТЬ
      </div>

      <div
        class="font-cormorant-sc q-mt-lg text-block"
        :class="nameSizeClass"
        :style="isMobile ? mobileTextStyle : placeStyle"
      >
        «KINZA»
      </div>

      <div
        class="font-cormorant-sc q-mt-lg text-block"
        :class="textSizeClass"
        :style="isMobile ? mobileTextStyle : addressStyle"
      >
        МИКРОРАЙОН ЦРБ, 2в
      </div>

      <div
        class="q-mt-xl text-block"
        :style="isMobile ? { textAlign: 'center' } : buttonStyle"
      >
        <q-btn
          rounded
          outline
          class="font-cormorant-sc"
          color="white"
          text-color="white"
          label="Показать на карте"
          :size="isMobile ? 'lg' : 'xl'"
          @click="openMap"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, computed, ref, onMounted, onUnmounted } from 'vue'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'PlaceComponent',
  setup() {
    const $q = useQuasar()
    const container = ref(null)
    const isVisible = ref(false)
    let observer = null

    const isMobile = computed(() => $q.screen.lt.md)
    const textSizeClass = computed(() => !$q.screen.lt.md ? 'text-h4' : 'text-h5')
    const nameSizeClass = computed(() => !$q.screen.lt.md ? 'text-h3' : 'text-h4')

    const locationStyle = { marginLeft: '180px', maxWidth: '600px' }
    const banquetStyle = { marginLeft: '190px' }
    const placeStyle = { marginLeft: '300px' }
    const addressStyle = { marginLeft: '220px' }
    const buttonStyle = { marginLeft: '246px' }
    const mobileTextStyle = { marginLeft: '0', maxWidth: '90%', margin: '0 auto' }

    const openMap = () => {
      window.open('https://2gis.kz/kokshetau/firm/70000001091595292', '_blank')
    }

    onMounted(() => {
      observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            isVisible.value = true
            if (observer) {
              observer.disconnect()
              observer = null
            }
          }
        })
      }, {
        threshold: 0.3,
        rootMargin: '50px'
      })

      if (container.value) {
        observer.observe(container.value)
      }
    })

    onUnmounted(() => {
      if (observer) {
        observer.disconnect()
      }
    })

    return {
      container,
      isVisible,
      isMobile,
      textSizeClass,
      nameSizeClass,
      locationStyle,
      banquetStyle,
      placeStyle,
      addressStyle,
      buttonStyle,
      mobileTextStyle,
      openMap
    }
  }
})
</script>

<style lang="scss" scoped>
.bg-image {
  background-image: url('/img/place.png');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  height: 1200px;
  width: 100%;
  opacity: 0;
  transition: opacity 0.8s ease-out;
}

.text-block {
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease-out;
}

.q-btn {
  transition: all 0.3s ease;

  &:hover {
    transform: scale(1.05);
    background-color: rgba(255, 255, 255, 0.1);
  }
}

.bg-image.animate {
  opacity: 1;

  .text-block:nth-child(1) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.2s;
  }

  .text-block:nth-child(2) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.4s;
  }

  .text-block:nth-child(3) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.6s;
  }

  .text-block:nth-child(4) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.8s;
  }

  .text-block:nth-child(5) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1s;
  }
}

// Адаптивность
@media (max-width: 768px) {
  .bg-image {
    height: 800px;
  }

  .bg-image.animate {
    .text-block:nth-child(1) { transition-delay: 0.1s; }
    .text-block:nth-child(2) { transition-delay: 0.2s; }
    .text-block:nth-child(3) { transition-delay: 0.3s; }
    .text-block:nth-child(4) { transition-delay: 0.4s; }
    .text-block:nth-child(5) { transition-delay: 0.5s; }
  }
}

@media (max-width: 480px) {
  .bg-image.animate {
    .text-block:nth-child(1) { transition-delay: 0.05s; }
    .text-block:nth-child(2) { transition-delay: 0.15s; }
    .text-block:nth-child(3) { transition-delay: 0.25s; }
    .text-block:nth-child(4) { transition-delay: 0.35s; }
    .text-block:nth-child(5) { transition-delay: 0.45s; }
  }
}
</style>
