<template>
  <div
    ref="container"
    class="details-container bg-image details-overlap"
    :class="{ 'animate': isVisible }"
  >
    <div class="text-h3 text-center font-cormorant-sc q-mb-lg q-mt-xl text-white">DETAILS</div>

    <div class="detail-block block-01">
      <div
        class="font-cormorant-infant text-white"
        :class="!$q.screen.lt.md ? 'text-h1' : 'text-h2'"
      >
        01
      </div>
      <div
        class="font-cormorant-sc q-mt-sm text-white"
        :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'"
      >
        Чтобы ваши руки были свободны для объятий, будем рады легким подаркам в конвертах
      </div>
    </div>

    <div class="detail-block block-02">
      <div
        class="font-cormorant-infant text-white"
        :class="!$q.screen.lt.md ? 'text-h1' : 'text-h2'"
      >
        02
      </div>
      <div
        class="font-cormorant-sc q-mt-sm text-white"
        :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'"
      >
        Приятным комплиментом для нас будет, если вместо цветов вы решите подарить нам бутылочку
        вина для нашей семейной винотеки
      </div>
    </div>

    <div class="detail-block block-03">
      <div
        class="font-cormorant-infant text-white"
        :class="!$q.screen.lt.md ? 'text-h1' : 'text-h2'"
      >
        03
      </div>
      <div
        class="font-cormorant-sc q-mt-sm text-white"
        :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'"
      >
        Обращаем ваше внимание, что мероприятие предназначено исключительно для взрослых гостей -
        десткий стол и аниматоры не предусмотрены
      </div>
    </div>

    <div class="image-bottom">
      <img src="/public/img/wedding.png" alt="Деталь" class="bottom-image" />
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'

export default defineComponent({
  name: 'detailsComponent',
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
          rootMargin: '50px',
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
.details-overlap {
  position: relative;
  z-index: 8;
  margin-top: -120px;
  padding-top: 140px;
  padding-bottom: 140px;
}

.bg-image {
  background-image: url('/img/details.png');
  background-repeat: no-repeat;
  background-position: center center;
  min-height: 1400px;
  display: flex;
  align-items: center;
  justify-content: center;

  /* Адаптивный подход */
  @media (min-width: 1920px) {
    background-size: 80% auto;
  }

  @media (min-width: 1440px) and (max-width: 1919px) {
    background-size: 90% auto;
  }

  @media (min-width: 1024px) and (max-width: 1439px) {
    background-size: 95% auto;
  }

  @media (min-width: 768px) and (max-width: 1023px) {
    background-size: 110% auto;
    min-height: 1200px;
  }

  @media (max-width: 767px) {
    background-size: 130% auto;
    min-height: 1000px;
  }

  @media (max-width: 480px) {
    background-size: 150% auto;
    min-height: 800px;
  }
}

.details-container {
  width: 100%;
}

.detail-block {
  width: 100%;
  display: flex;
  flex-direction: column;
  padding: 80px 10%;
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s ease;
}

.block-01 {
  align-items: flex-start;
  text-align: left;
  transition-delay: 0.2s;
}

.block-02 {
  align-items: flex-end;
  text-align: right;
  transition-delay: 0.4s;
}

.block-03 {
  align-items: flex-start;
  text-align: left;
  transition-delay: 0.6s;
}

.image-bottom {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 80px 0;
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s ease 0.8s;
}

.bottom-image {
  max-width: 800px;
  width: 100%;
  height: auto;
}

.details-container.animate {
  .detail-block {
    opacity: 1;
    transform: translateY(0);
  }

  .image-bottom {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .details-overlap {
    margin-top: -80px;
    padding-top: 100px;
    padding-bottom: 100px;
  }

  .detail-block {
    padding: 60px 5%;
  }

  .image-bottom {
    padding: 60px 0;
  }

  .block-01 {
    transition-delay: 0.1s;
  }

  .block-02 {
    transition-delay: 0.2s;
  }

  .block-03 {
    transition-delay: 0.3s;
  }

  .image-bottom {
    transition-delay: 0.4s;
  }
}

@media (max-width: 480px) {
  .details-overlap {
    margin-top: -60px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .detail-block {
    padding: 40px 20px;
  }

  .image-bottom {
    padding: 40px 0;
  }

  .text-h3 {
    font-size: 2rem;
  }
}
</style>
