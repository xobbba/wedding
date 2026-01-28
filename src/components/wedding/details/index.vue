<template>
  <div ref="container" class="details-container bg-image q-mt-xl q-mb-xl">
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
.bg-image {
  background-image: url('/img/timing.png');
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
  width: 100%;
  min-height: 1200px;
}

.details-container {
  width: 100%;
  padding-top: 40px; /* Добавляем отступ сверху */
  padding-bottom: 40px; /* Добавляем отступ снизу */
}

.detail-block {
  width: 100%;
  display: flex;
  flex-direction: column;
  padding: 80px 10%;
}

.block-01 {
  align-items: flex-start;
  text-align: left;
}

.block-02 {
  align-items: flex-end;
  text-align: right;
}

.block-03 {
  align-items: flex-start;
  text-align: left;
}

.image-bottom {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 80px 0;
}

.bottom-image {
  max-width: 800px;
  width: 100%;
  height: auto;
}

/* Медиа-запросы для адаптивности */
@media (max-width: 768px) {
  .details-container {
    padding-top: 20px;
    padding-bottom: 20px;
  }

  .detail-block {
    padding: 60px 5%;
  }

  .image-bottom {
    padding: 60px 0;
  }
}

@media (max-width: 480px) {
  .details-container {
    padding-top: 10px;
    padding-bottom: 10px;
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
