<template>
  <div
    ref="container"
    class="row items-center justify-center timing-overlap"
    :class="{ 'animate': isVisible }"
  >
    <div class="bg-image"></div>
    <div class="text-white text-center content" :class="{
      'q-mt-xl q-mb-xl': $q.screen.lt.md
    }">
      <div class="font-cormorant-sc q-mb-xl timing-title" :class="!$q.screen.lt.md ? 'text-h3' : 'text-h4'">
        TIMING
      </div>

      <div class="centered-container q-mt-xl">
        <div class="timing-item q-pa-md" data-index="0">
          <div class="font-cormorant-infant" :class="!$q.screen.lt.md ? 'text-h3' : 'text-h4'">15:00</div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
            ЦЕРЕМОНИЯ БРАКОСОЧЕТАНИЯ
          </div>
          <div class="font-cormorant-sc q-mt-md q-mr-xl q-ml-xl" :class="!$q.screen.lt.md ? 'text-h5' : 'text-body1'">
            <span v-if="!$q.screen.lt.md">
              ЦЕРЕМОНИЯ СОСТОИТСЯ В «АЙНАКӨЛ»<br />
              ПО АДРЕСУ: АК-ЖАЙЫК, 1/3<br />
              КАЗАХСТАН, БОРОВОЕ.
            </span>
            <span v-else>
              ЦЕРЕМОНИЯ СОСТОИТСЯ В «АЙНАКӨЛ» ПО АДРЕСУ: АК-ЖАЙЫК, 1/3. КАЗАХСТАН,
              БОРОВОЕ.
            </span>
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="1">
          <div class="font-cormorant-infant" :class="!$q.screen.lt.md ? 'text-h3' : 'text-h4'">16:30</div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
            НАЧАЛО ФУРШЕТА
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="2">
          <div class="font-cormorant-infant" :class="!$q.screen.lt.md ? 'text-h3' : 'text-h4'">17:30</div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
            НАЧАЛО БАНКЕТА И ВСТРЕЧА МОЛОДОЖЕНОВ
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="3">
          <div class="font-cormorant-infant" :class="!$q.screen.lt.md ? 'text-h3' : 'text-h4'">23:00</div>
          <div
            class="font-cormorant-sc q-mt-sm q-mb-xl"
            :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'"
          >
            ОКОНЧАНИЕ БАНКЕТА
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'

export default defineComponent({
  name: 'TimingComponent',
  setup() {
    const container = ref(null)
    const isVisible = ref(false)
    let observer = null

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
      if (observer) observer.disconnect()
    })

    return {
      container,
      isVisible,
    }
  },
})
</script>

<style lang="scss" scoped>
.timing-overlap {
  position: relative;
  z-index: 6;
  margin-top: -120px;
  padding-top: 140px;
  padding-bottom: 140px;
  overflow: hidden;
}

.bg-image {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('/img/timing.png');
  background-repeat: no-repeat;
  background-position: center center;
  opacity: 1;
  will-change: opacity;
  background-size: cover;

  @media (max-width: 2200px) {
    background-size: 107% 96%;
    background-position: center 40%;
  }

  @media (max-width: 1950px) {
    background-size: 110% auto;
  }

  @media (max-width: 1800px) {
    background-size: 120% auto;
  }

  @media (max-width: 1600px) {
    background-size: 140% auto;
  }

  @media (max-width: 1450px) {
    background-size: 160% auto;
  }

  @media (max-width: 1200px) {
    background-size: 200% auto;
  }

  @media (max-width: 1023px) {
    background-size: 450% auto;
  }

  @media (max-width: 400px) {
    background-size: 450% auto;
  }

  @media screen and (max-width: 1400px) and (min-height: 1020px) {
    background-size: 450% auto;
  }
}

.content {
  position: relative;
  z-index: 2;
  width: 100%;
}

.centered-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.timing-item {
  width: 100%;
  max-width: 800px;
  text-align: center;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease, transform 0.6s ease;
  will-change: transform, opacity;
}

.separator-center {
  display: flex;
  justify-content: center;
  width: 100%;
  margin: 2px 0;
}

.centered-separator {
  height: 60px;
  width: 2px;
  opacity: 0;
  transition: opacity 0.6s ease;
  will-change: opacity;
}

.timing-title {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease, transform 0.6s ease;
  will-change: transform, opacity;
}

.timing-overlap.animate {
  .timing-title {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.2s;
  }

  .timing-item[data-index='0'] {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.4s;
  }

  .centered-separator:nth-of-type(1) {
    opacity: 0.7;
    transition-delay: 0.55s;
  }

  .timing-item[data-index='1'] {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.7s;
  }

  .centered-separator:nth-of-type(2) {
    opacity: 0.7;
    transition-delay: 0.85s;
  }

  .timing-item[data-index='2'] {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1s;
  }

  .centered-separator:nth-of-type(3) {
    opacity: 0.7;
    transition-delay: 1.15s;
  }

  .timing-item[data-index='3'] {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1.3s;
  }
}

@media (max-width: 1024px) {
  .timing-overlap {
    margin-top: -100px;
    padding-top: 120px;
    padding-bottom: 120px;
  }

  .timing-overlap.animate {
    .timing-title {
      transition-delay: 0.1s;
    }

    .timing-item[data-index='0'] {
      transition-delay: 0.3s;
    }

    .centered-separator:nth-of-type(1) {
      transition-delay: 0.4s;
    }

    .timing-item[data-index='1'] {
      transition-delay: 0.5s;
    }

    .centered-separator:nth-of-type(2) {
      transition-delay: 0.6s;
    }

    .timing-item[data-index='2'] {
      transition-delay: 0.7s;
    }

    .centered-separator:nth-of-type(3) {
      transition-delay: 0.8s;
    }

    .timing-item[data-index='3'] {
      transition-delay: 0.9s;
    }
  }
}

@media (max-width: 768px) {
  .timing-overlap {
    margin-top: -80px;
    padding-top: 100px;
    padding-bottom: 100px;
  }

  .timing-overlap.animate {
    .timing-item[data-index='0'] {
      transition-delay: 0.2s;
    }

    .centered-separator:nth-of-type(1) {
      transition-delay: 0.3s;
    }

    .timing-item[data-index='1'] {
      transition-delay: 0.4s;
    }

    .centered-separator:nth-of-type(2) {
      transition-delay: 0.5s;
    }

    .timing-item[data-index='2'] {
      transition-delay: 0.6s;
    }

    .centered-separator:nth-of-type(3) {
      transition-delay: 0.7s;
    }

    .timing-item[data-index='3'] {
      transition-delay: 0.8s;
    }
  }

  .centered-separator {
    height: 50px;
  }
}

@media (max-width: 480px) {
  .timing-overlap {
    margin-top: -60px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .timing-item {
    padding: 10px;
  }

  .centered-separator {
    height: 40px;
  }

  .bg-image {
    opacity: 1;
  }
}

@media (prefers-reduced-motion: reduce) {
  .bg-image,
  .timing-title,
  .timing-item,
  .centered-separator {
    transition: none !important;
  }

  .timing-overlap.animate {
    .bg-image {
      opacity: 1;
    }

    .timing-title,
    .timing-item,
    .centered-separator {
      opacity: 1;
      transform: none;
    }
  }
}
</style>
