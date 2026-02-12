<template>
  <div class="full-container">
    <div
      ref="container"
      class="row items-center justify-start bg-image place-overlap"
      :class="{ animate: isVisible }"
    >
      <div class="text-white" :class="{ 'text-center': isMobile }">
        <div
          class="font-cormorant-sc q-mt-md text-block"
          :class="textSizeClass"
          :style="isMobile ? mobileTextStyle : locationStyle"
        >
          НАША СВАДЬБА ПРОЙДЕТ В КАЗАХСТАНЕ, БОРОВОЕ
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
          «Wyndham Garden»
        </div>

        <div
          class="font-cormorant-sc q-mt-lg text-block"
          :class="textSizeClass"
          :style="isMobile ? mobileTextStyle : addressStyle"
        >
          МИКРОРАЙОН ЦРБ, 2в
        </div>

        <div class="q-mt-xl text-block" :style="isMobile ? { textAlign: 'center' } : buttonStyle">
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
    const textSizeClass = computed(() => (!$q.screen.lt.md ? 'text-h4' : 'text-h5'))
    const nameSizeClass = computed(() => (!$q.screen.lt.md ? 'text-h3' : 'text-h4'))

    const locationStyle = { marginLeft: '190px', maxWidth: '600px' }
    const banquetStyle = { marginLeft: '190px' }
    const placeStyle = { marginLeft: '210px' }
    const addressStyle = { marginLeft: '224px' }
    const buttonStyle = { marginLeft: '266px' }
    const mobileTextStyle = { marginLeft: '0', maxWidth: '90%', margin: '0 auto' }

    const openMap = () => {
      window.open(
        'https://2gis.kz/kokshetau/firm/70000001056259878/70.30477%2C53.079132?m=70.310225%2C53.076573%2F13.89',
        '_blank',
      )
    }

    onMounted(() => {
      observer = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            if (entry.isIntersecting) {
              isVisible.value = true
              if (observer) {
                observer.disconnect()
                observer = null
              }
            }
          })
        },
        {
          threshold: 0.3,
          rootMargin: '50px',
        },
      )

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
      openMap,
    }
  },
})
</script>

<style lang="scss" scoped>
.full-container {
  position: relative;
  width: 100%;
}

.place-overlap {
  position: relative;
  z-index: 5;
  margin-top: -100px;
  padding-top: 120px;
  padding-bottom: 120px;
}

.bg-image {
  background-image: url('/img/place.jpg');
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

@media (max-width: 768px) {
  .place-overlap {
    margin-top: -80px;
    padding-top: 100px;
    padding-bottom: 100px;
  }

  .bg-image {
    height: 800px;
  }

  .bg-image.animate {
    .text-block:nth-child(1) {
      transition-delay: 0.1s;
    }
    .text-block:nth-child(2) {
      transition-delay: 0.2s;
    }
    .text-block:nth-child(3) {
      transition-delay: 0.3s;
    }
    .text-block:nth-child(4) {
      transition-delay: 0.4s;
    }
    .text-block:nth-child(5) {
      transition-delay: 0.5s;
    }
  }
}

@media (max-width: 480px) {
  .place-overlap {
    margin-top: -60px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .bg-image.animate {
    .text-block:nth-child(1) {
      transition-delay: 0.05s;
    }
    .text-block:nth-child(2) {
      transition-delay: 0.15s;
    }
    .text-block:nth-child(3) {
      transition-delay: 0.25s;
    }
    .text-block:nth-child(4) {
      transition-delay: 0.35s;
    }
    .text-block:nth-child(5) {
      transition-delay: 0.45s;
    }
  }
}
</style>
