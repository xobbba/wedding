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
          НАША СВАДЬБА ПРОЙДЕТ В КАЗАХСТАНЕ, Г. КОКШЕТАУ
        </div>

        <div
          class="font-cormorant-sc q-mt-lg text-block"
          :class="nameSizeClass"
          :style="isMobile ? mobileTextStyle : placeStyle"
        >
          «ROYAL»
        </div>

        <div
          class="font-cormorant-sc q-mt-lg text-block"
          :class="placeSizeClass"
          :style="isMobile ? mobileTextStyle : addressStyle"
        >
          АЛИМЖАНА БАЙМУКАНОВА 61В
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
    const textSizeClass = computed(() => (!$q.screen.lt.md ? 'text-h4' : 'text-h6'))
    const placeSizeClass = computed(() => (!$q.screen.lt.md ? 'text-h5' : 'text-subtitle1'))
    const nameSizeClass = computed(() => (!$q.screen.lt.md ? 'text-h3' : 'text-h5'))

    const locationStyle = { marginLeft: '190px', maxWidth: '600px' }
    const placeStyle = { marginLeft: '310px' }
    const addressStyle = { marginLeft: '216px' }
    const buttonStyle = { marginLeft: '266px' }
    const mobileTextStyle = { marginLeft: '0', maxWidth: '90%', margin: '0 auto' }

    const openMap = () => {
      window.open('https://2gis.kz/kokshetau/firm/70000001033670090', '_blank')
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
          threshold: 0.2,
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
      placeSizeClass,
      nameSizeClass,
      locationStyle,
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
  background-image: url('/img/place.JPG');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  height: 900px;
  width: 100%;
  opacity: 1;
  transition: opacity 0.8s ease-out;

  @media (max-width: 768px) {
    background-image: url('/img/place2.JPG');
    background-position: 30% center;
  }

  @media (max-width: 480px) {
    background-position: 25% center;
  }
}

.text-block {
  opacity: 0;
  transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
  will-change: transform, opacity;
}

.text-block:nth-child(1) {
  transform: translateX(-50px);
}

.text-block:nth-child(2) {
  transform: translateX(50px);
}

.text-block:nth-child(3) {
  transform: translateX(-50px);
}

.text-block:nth-child(4) {
  transform: translateY(30px);
}

.q-btn {
  transition: all 0.3s ease;

  &:hover {
    transform: scale(1.05);
    background-color: rgba(255, 255, 255, 0.1);
  }
}

.bg-image.animate {
  .text-block:nth-child(1) {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.2s;
  }

  .text-block:nth-child(2) {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.4s;
  }

  .text-block:nth-child(3) {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.6s;
  }

  .text-block:nth-child(4) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.8s;
  }
}

@media (max-width: 1024px) {
  .place-overlap {
    margin-top: -100px;
    padding-top: 120px;
    padding-bottom: 120px;
  }

  .bg-image {
    height: 850px;
  }

  .bg-image.animate {
    .text-block:nth-child(1) {
      transition-delay: 0.15s;
    }
    .text-block:nth-child(2) {
      transition-delay: 0.3s;
    }
    .text-block:nth-child(3) {
      transition-delay: 0.45s;
    }
    .text-block:nth-child(4) {
      transition-delay: 0.6s;
    }
  }

  .text-block:nth-child(1),
  .text-block:nth-child(3) {
    transform: translateX(-30px);
  }

  .text-block:nth-child(2) {
    transform: translateX(30px);
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
  }

  .text-block:nth-child(1),
  .text-block:nth-child(2),
  .text-block:nth-child(3) {
    transform: translateY(20px);
  }

  .bg-image.animate {
    .text-block:nth-child(1),
    .text-block:nth-child(2),
    .text-block:nth-child(3) {
      transform: translateY(0);
    }
  }
}

@media (max-width: 480px) {
  .place-overlap {
    margin-top: -60px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .bg-image {
    background-position: 20% center;
    height: 700px;
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
  }
}

@media (prefers-reduced-motion: reduce) {
  .bg-image,
  .text-block {
    transition: none !important;
  }

  .bg-image.animate {
    .text-block {
      opacity: 1;
      transform: none;
    }
  }
}
</style>
