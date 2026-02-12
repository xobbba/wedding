<template>
  <div
    ref="container"
    class="row items-center justify-center bg-image timing-overlap"
    :class="{ 'animate': isVisible }"
  >
    <div class="text-white text-center" :class="{
      'q-mt-xl q-mb-xl': $q.screen.lt.md
    }">
      <div class="text-h3 font-cormorant-sc q-mb-xl timing-title">
        TIMING
      </div>

      <div class="centered-container q-mt-xl">
        <div class="timing-item q-pa-md" data-index="0">
          <div class="text-h3 font-cormorant-infant">15:00</div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
            ЦЕРЕМОНИЯ БРАКОСОЧЕТАНИЯ
          </div>
          <div class="font-cormorant-sc q-mt-md" :class="!$q.screen.lt.md ? 'text-h5' : 'text-h6'">
            <span v-if="!$q.screen.lt.md">
              ЦЕРЕМОНИЯ СОСТОИТСЯ В «BURABAY GOLF CLUB»<br />
              ПО АДРЕСУ: МИКРОРАЙОН ЦРБ, 2Б.<br />
              КАЗАХСТАН, Г. ЩУЧИНСК.
            </span>
            <span v-else>
              ЦЕРЕМОНИЯ СОСТОИТСЯ В «BURABAY GOLF CLUB» ПО АДРЕСУ: МИКРОРАЙОН ЦРБ, 2Б. КАЗАХСТАН, Г.
              ЩУЧИНСК.
            </span>
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="1">
          <div class="text-h3 font-cormorant-infant">16:30</div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
            НАЧАЛО ФУРШЕТА
          </div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.md ? 'text-h5' : 'text-h6'">
            <span v-if="!$q.screen.lt.md">
              Мухтара Ауэзова, 129<br />
              «KINZA»
            </span>
            <span v-else> Мухтара Ауэзова, 129 «KINZA» </span>
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="2">
          <div class="text-h3 font-cormorant-infant">17:30</div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
            НАЧАЛО БАНКЕТА И ВСТРЕЧА МОЛОДОЖЕНОВ
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="3">
          <div class="text-h3 font-cormorant-infant">23:00</div>
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
}

.bg-image {
  background-image: url('/img/timing.png');
  background-repeat: no-repeat;
  background-position: center center;
  min-height: 1400px;
  display: flex;
  align-items: center;
  justify-content: center;

  /* Адаптивный подход */
  @media (min-width: 1920px) {
    background-size: 120% auto;
  }

  @media (min-width: 1440px) and (max-width: 1919px) {
    background-size: 176% auto;
  }

  @media (min-width: 1024px) and (max-width: 1439px) {
    background-size: 214% auto;
  }

  @media (min-width: 768px) and (max-width: 1023px) {
    background-size: 260% auto;
    min-height: 1200px;
  }

  @media (max-width: 767px) {
    background-size: 325% auto;
    min-height: 1000px;
  }

  @media (max-width: 480px) {
    background-size: 594% auto;
    min-height: 800px;
  }
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
  transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);

  &:hover {
    transform: translateY(-2px);
  }
}

.separator-center {
  display: flex;
  justify-content: center;
  width: 100%;
  margin: 20px 0;
}

.centered-separator {
  height: 60px;
  width: 2px;
  opacity: 0;
  transform: scaleY(0);
  transform-origin: top center;
  transition: all 0.6s ease;
}

.timing-title {
  opacity: 0;
  transform: scale(0.9);
  transition: all 0.8s ease;
}

.bg-image.animate {
  .timing-title {
    opacity: 1;
    transform: scale(1);
  }

  .timing-item[data-index='0'] {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.2s;
  }

  .timing-item[data-index='1'] {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.5s;
  }

  .timing-item[data-index='2'] {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.8s;
  }

  .timing-item[data-index='3'] {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1.1s;
  }

  .centered-separator {
    opacity: 0.7;
    transform: scaleY(1);

    &:nth-child(2) {
      transition-delay: 0.35s;
    }

    &:nth-child(4) {
      transition-delay: 0.65s;
    }

    &:nth-child(6) {
      transition-delay: 0.95s;
    }
  }
}

@media (max-width: 768px) {
  .timing-overlap {
    margin-top: -80px;
    padding-top: 100px;
    padding-bottom: 100px;
  }

  .bg-image.animate {
    .timing-item[data-index='0'] {
      transition-delay: 0.1s;
    }
    .timing-item[data-index='1'] {
      transition-delay: 0.3s;
    }
    .timing-item[data-index='2'] {
      transition-delay: 0.5s;
    }
    .timing-item[data-index='3'] {
      transition-delay: 0.7s;
    }

    .centered-separator:nth-child(2) {
      transition-delay: 0.2s;
    }
    .centered-separator:nth-child(4) {
      transition-delay: 0.4s;
    }
    .centered-separator:nth-child(6) {
      transition-delay: 0.6s;
    }
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
}
</style>
