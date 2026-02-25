<template>
  <div ref="container" class="details-container details-overlap" :class="{ animate: isVisible }">
    <div class="bg-image"></div>
    <div class="content-wrapper">
      <div
        class="text-center font-cormorant-sc q-mb-lg q-mt-xl text-white details-title"
        :class="!$q.screen.lt.md ? 'text-h3' : 'text-h4'"
      >
        DETAILS
      </div>

      <div class="detail-block block-01">
        <div
          class="font-cormorant-infant text-white"
          :class="!$q.screen.lt.md ? 'text-h1' : 'text-h2'"
        >
          01
        </div>
        <div
          class="font-cormorant-sc q-mt-sm text-white"
          :class="!$q.screen.lt.md ? 'text-h4' : 'text-h6'"
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
          :class="!$q.screen.lt.md ? 'text-h4' : 'text-h6'"
        >
          Наша свадьба состоится в другой стране и мы к сожалению, не сможем забрать цветы домой.
          Будем рады другим подаркам, которые останутся с нами навсегда
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
          :class="!$q.screen.lt.md ? 'text-h4' : 'text-h6'"
        >
          Церемония бракосочетания и банкет пройдут в разных локациях. Чтобы вы могли расслабиться и
          насладиться праздником, мы организовали трансфер от места церемонии до банкетного зала и
          обратно
        </div>
      </div>

      <div class="detail-block block-02">
        <div
          class="font-cormorant-infant text-white"
          :class="!$q.screen.lt.md ? 'text-h1' : 'text-h2'"
        >
          04
        </div>
        <div
          class="font-cormorant-sc q-mt-sm text-white"
          :class="!$q.screen.lt.md ? 'text-h4' : 'text-h6'"
        >
          Обращаем ваше внимание, что мероприятие предназначено исключительно для взрослых гостей -
          детский стол и аниматоры не предусмотрены
        </div>
      </div>
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
  overflow: hidden;
}

.bg-image {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('/img/details.png');
  background-repeat: no-repeat;
  background-position: center center;
  opacity: 1;
  transform: scale(1);
  will-change: transform, opacity;
  background-size: cover;

  @media (max-width: 1950px) {
    background-size: 140% auto;
  }

  @media (max-width: 1800px) {
    background-size: 155% auto;
  }

  @media (max-width: 1600px) {
    background-size: 170% auto;
  }

  @media (max-width: 1450px) {
    background-size: 190% auto;
  }

  @media (max-width: 1200px) {
    background-size: 210% auto;
  }

  @media (max-width: 1023px) {
    background-size: 550% auto;
  }

  @media (max-width: 400px) {
    background-size: 550% auto;
  }

  @media screen and (max-width: 1400px) and (min-height: 1020px) {
    background-size: 550% auto;
  }
}

.content-wrapper {
  position: relative;
  z-index: 2;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.details-title {
  opacity: 0;
  transform: translateY(-30px);
  transition:
    opacity 0.6s ease,
    transform 0.6s ease;
  will-change: transform, opacity;
}

.detail-block {
  width: 100%;
  max-width: 1200px;
  display: flex;
  flex-direction: column;
  padding: 20px 10%;
  box-sizing: border-box;
  opacity: 0;
  transition:
    opacity 0.6s ease,
    transform 0.6s ease;
  will-change: transform, opacity;
}

.block-01 {
  align-items: flex-start;
  text-align: left;
  align-self: flex-start;
  transform: translateX(-50px);
}

.block-02 {
  align-items: flex-end;
  text-align: right;
  align-self: flex-end;
  transform: translateX(50px);
}

.block-03 {
  align-items: flex-start;
  text-align: left;
  align-self: flex-start;
  transform: translateX(-50px);
}

.image-bottom {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 80px 0;
  opacity: 0;
  transform: translateY(30px);
  transition:
    opacity 0.6s ease,
    transform 0.6s ease;
  will-change: transform, opacity;
}

.bottom-image {
  max-width: 800px;
  width: 100%;
  height: auto;
}

.details-container.animate {
  .details-title {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.2s;
  }

  .block-01 {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.4s;
  }

  .block-02 {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.6s;
  }

  .block-03 {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.8s;
  }

  .image-bottom {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1s;
  }
}

@media (max-width: 1024px) {
  .details-overlap {
    margin-top: -100px;
    padding-top: 120px;
    padding-bottom: 120px;
  }

  .details-container.animate {
    .details-title {
      transition-delay: 0.1s;
    }

    .block-01 {
      transition-delay: 0.3s;
    }
    .block-02 {
      transition-delay: 0.5s;
    }
    .block-03 {
      transition-delay: 0.7s;
    }
    .image-bottom {
      transition-delay: 0.9s;
    }
  }

  .block-01,
  .block-03 {
    transform: translateX(-30px);
  }

  .block-02 {
    transform: translateX(30px);
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

  .details-container.animate {
    .block-01 {
      transition-delay: 0.2s;
    }
    .block-02 {
      transition-delay: 0.4s;
    }
    .block-03 {
      transition-delay: 0.6s;
    }
    .image-bottom {
      transition-delay: 0.8s;
    }
  }

  .block-01,
  .block-03 {
    transform: translateX(-20px);
  }

  .block-02 {
    transform: translateX(20px);
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

  .bg-image {
    opacity: 1;
    transform: scale(1);
  }

  .block-01,
  .block-02,
  .block-03 {
    transform: translateY(20px);
  }

  .details-container.animate {
    .block-01,
    .block-02,
    .block-03 {
      transform: translateY(0);
    }
  }
}

@media (prefers-reduced-motion: reduce) {
  .bg-image,
  .details-title,
  .detail-block,
  .image-bottom {
    transition: none !important;
  }

  .details-container.animate {
    .bg-image {
      opacity: 1;
    }

    .details-title,
    .detail-block,
    .image-bottom {
      opacity: 1;
      transform: none;
    }
  }
}
</style>
