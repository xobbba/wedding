<template>
  <div
    ref="container"
    class="row items-center justify-center timing-overlap"
    :class="{ animate: isVisible }"
  >
    <div class="bg-image"></div>
    <div
      class="text-white text-center content"
      :class="{
        'q-mt-xl q-mb-xl': $q.screen.lt.md,
      }"
    >
      <div
        class="font-cormorant-sc q-mb-xl timing-title"
        :class="!$q.screen.lt.sm ? 'text-h3' : 'text-h4'"
      >
        TIMING
      </div>

      <div class="centered-container q-mt-xl">
        <div class="timing-item q-pa-md" data-index="0">
          <div class="font-cormorant-infant" :class="!$q.screen.lt.sm ? 'text-h3' : 'text-h4'">
            14:00
          </div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.sm ? 'text-h4' : 'text-h5'">
            ЦЕРЕМОНИЯ БРАКОСОЧЕТАНИЯ
          </div>
          <div
            class="font-cormorant-sc q-mt-md q-mr-xl q-ml-xl"
            :class="!$q.screen.lt.sm ? 'text-h5' : 'text-body1'"
          >
            <span v-if="!$q.screen.lt.sm">
              ЦЕРЕМОНИЯ СОСТОИТСЯ В
              <a
                href="https://2gis.kz/kokshetau/firm/70000001054929415/70.231365%2C52.955337"
                target="_blank"
                rel="noopener noreferrer"
                class="text-white location-link"
              >
                «BURABAY ГОЛЬФ-КЛУБ» </a
              ><br />
              ПО АДРЕСУ: МИКРОРАЙОН ЦРБ, 2Б<br />
              КАЗАХСТАН, ЩУЧИНСК.
            </span>
            <span v-else>
              ЦЕРЕМОНИЯ СОСТОИТСЯ В
              <a
                href="https://2gis.kz/kokshetau/firm/70000001054929415/70.231365%2C52.955337"
                target="_blank"
                rel="noopener noreferrer"
                class="text-white location-link"
              >
                «BURABAY ГОЛЬФ-КЛУБ»
              </a>
              ПО АДРЕСУ: МИКРОРАЙОН ЦРБ, 2Б КАЗАХСТАН, ЩУЧИНСК.
            </span>
            <div
              class="q-mt-md transfer-box font-cormorant-sc"
              :class="!$q.screen.lt.sm ? 'text-h6' : 'text-body1'"
              style="opacity: 0.7"
            >
              Для тех, кто не сможет присутствовать на церемонии, в 15:30 вас будет ждать трансфер
            </div>
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="1">
          <div class="font-cormorant-infant" :class="!$q.screen.lt.sm ? 'text-h3' : 'text-h4'">
            15:40
          </div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.sm ? 'text-h4' : 'text-h5'">
            СБОР ГОСТЕЙ НА ТРАНСФЕР ДО МЕСТА ПРОВЕДЕНИЯ БАНКЕТА
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="2">
          <div class="font-cormorant-infant" :class="!$q.screen.lt.sm ? 'text-h3' : 'text-h4'">
            17:00
          </div>
          <div class="font-cormorant-sc q-mt-sm" :class="!$q.screen.lt.sm ? 'text-h4' : 'text-h5'">
            НАЧАЛО БАНКЕТА И ВСТРЕЧА МОЛОДОЖЕНОВ
          </div>
          <div
            class="font-cormorant-sc q-mt-md q-mr-xl q-ml-xl"
            :class="!$q.screen.lt.sm ? 'text-h5' : 'text-body1'"
          >
            <span v-if="!$q.screen.lt.sm">
              БАНКЕТ СОСТОИТСЯ В «ROYAL»<br />
              ПО АДРЕСУ: АЛИМЖАНА БАЙМУКАНОВА 61В<br />
              КАЗАХСТАН, КОКШЕТАУ.
            </span>
            <span v-else>
              БАНКЕТ СОСТОИТСЯ В «ROYAL» ПО АДРЕСУ: АЛИМЖАНА БАЙМУКАНОВА 61В. КАЗАХСТАН, КОКШЕТАУ.
            </span>
          </div>
        </div>

        <div class="separator-center">
          <q-separator vertical dark class="centered-separator" />
        </div>

        <div class="timing-item" data-index="3">
          <div class="font-cormorant-infant" :class="!$q.screen.lt.sm ? 'text-h3' : 'text-h4'">
            23:30
          </div>
          <div
            class="font-cormorant-sc q-mt-sm q-mb-xl"
            :class="!$q.screen.lt.sm ? 'text-h4' : 'text-h5'"
          >
            ЗАВЕРШЕНИЕ ВЕЧЕРА
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
  background-size: cover;
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
  transition:
    opacity 0.6s ease,
    transform 0.6s ease;
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
}

.timing-title {
  opacity: 0;
  transform: translateY(30px);
  transition:
    opacity 0.6s ease,
    transform 0.6s ease;
}

.location-link {
  text-decoration: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.5);
  transition: all 0.3s ease;
  display: inline-block;

  &:hover {
    border-bottom-color: #ffffff;
    transform: scale(1.02);
    text-shadow: 0 0 8px rgba(255, 255, 255, 0.5);
  }
}

.transfer-box {
  border-radius: 60px;
  padding: 12px 24px;
  display: inline-block;
  width: 620px;
  max-width: 100%;
  margin-left: auto;
  margin-right: auto;
  color: white;
  opacity: 0.9;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 245, 215, 0.4);
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

  .centered-separator {
    height: 50px;
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
    margin-top: -100px;
    padding-top: 100px;
    padding-bottom: 100px;
  }

  .centered-separator {
    height: 40px;
  }

  .timing-item {
    padding: 10px;
  }

  .transfer-box {
    padding: 10px 18px;
    border-radius: 50px;
    width: 100%;
    max-width: 100%;
  }
}

@media (max-width: 480px) {
  .timing-overlap {
    margin-top: -100px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .centered-separator {
    height: 30px;
  }

  .transfer-box {
    width: 100%;
    max-width: 100%;
    padding: 10px 16px;
    border-radius: 40px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .timing-title,
  .timing-item,
  .centered-separator {
    transition: none !important;
  }

  .timing-overlap.animate {
    .timing-title,
    .timing-item,
    .centered-separator {
      opacity: 1;
      transform: none;
    }
  }
}
</style>
