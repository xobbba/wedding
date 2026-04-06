<template>
  <div
    ref="container"
    class="row items-center justify-center invite-overlap"
    :class="{ animate: isVisible }"
  >
    <div class="bg-image"></div>
    <div class="text-white text-center q-pa-lg content">
      <div
        class="font-cormorant-sc q-mb-lg text-block"
        :class="!$q.screen.lt.sm ? 'text-h3' : 'text-h4'"
      >
        <span v-if="!$q.screen.lt.md"> ДОРОГИЕ РОДНЫЕ<br />И БЛИЗКИЕ! </span>
        <span v-else> ДОРОГИЕ РОДНЫЕ И БЛИЗКИЕ! </span>
      </div>

      <div
        class="font-cormorant-sc q-mb-md text-block"
        :class="[!$q.screen.lt.sm ? 'text-h4' : 'text-h6']"
      >
        <span v-if="!$q.screen.lt.md">
          Мы давно ждали этого момента, когда сможем<br />разделить с вами этот важный и
          счастливый<br />день в нашей жизни.
        </span>
        <span v-else>
          Мы давно ждали этого момента, когда сможем разделить с вами этот важный и счастливый день
          в нашей жизни.
        </span>
      </div>

      <div
        class="font-cormorant-sc q-mb-md text-block"
        :class="[!$q.screen.lt.sm ? 'text-h4' : 'text-h6']"
      >
        <span v-if="!$q.screen.lt.md"> Совсем скоро состоится<br />наша свадьба! </span>
        <span v-else> Совсем скоро состоится наша свадьба! </span>
      </div>

      <div
        class="font-cormorant-sc q-mb-lg text-block"
        :class="[!$q.screen.lt.sm ? 'text-h4' : 'text-h6']"
      >
        <span v-if="!$q.screen.lt.md">
          Мы рады пригласить вас стать<br />свидетелями этого торжества и разделить с нами<br />самые
          яркие моменты
        </span>
        <span v-else>
          Мы рады пригласить вас стать свидетелями этого торжества и разделить с нами самые яркие
          моменты
        </span>
      </div>

      <div class="calendar-container" :class="{ 'mobile-center': $q.screen.lt.sm }">
        <img src="/img/calendar.png" alt="Календарь" class="calendar-image" />
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'

export default defineComponent({
  name: 'InviteComponent',
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
              observer.disconnect()
            }
          })
        },
        {
          threshold: 0.2,
          rootMargin: '0px',
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
.invite-overlap {
  position: relative;
  z-index: 6;
  margin-top: -100px;
  padding-top: 120px;
  padding-bottom: 120px;
  overflow: hidden;
}

.bg-image {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('/img/invite.png');
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}

.content {
  position: relative;
  z-index: 2;
}

.text-block {
  opacity: 0;
  transform: translateY(30px);
  transition:
    opacity 0.6s ease,
    transform 0.6s ease;
}

.calendar-container {
  max-width: 700px;
  margin-left: 80px;
  opacity: 0;
  transform: translateY(30px);
  transition:
    opacity 0.8s ease,
    transform 0.8s ease;
}

.calendar-image {
  max-width: 100%;
  width: 100%;
  height: auto;
  transition: transform 0.5s ease;

  &:hover {
    transform: scale(1.03) translateY(-5px);
  }
}

/* Анимация */
.invite-overlap.animate {
  .text-block:nth-child(1) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.3s;
  }

  .text-block:nth-child(2) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.5s;
  }

  .text-block:nth-child(3) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.7s;
  }

  .text-block:nth-child(4) {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.9s;
  }

  .calendar-container {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1.2s;
  }
}

/* ========== УПРОЩЁННАЯ АДАПТИВНОСТЬ ========== */

/* Планшеты и ноутбуки (до 1024px) */
@media (max-width: 1024px) {
  .invite-overlap {
    margin-top: -100px;
    padding-top: 100px;
    padding-bottom: 100px;
  }

  .calendar-container {
    max-width: 550px;
    margin-left: 60px;
  }

  .invite-overlap.animate {
    .text-block:nth-child(1) {
      transition-delay: 0.2s;
    }
    .text-block:nth-child(2) {
      transition-delay: 0.3s;
    }
    .text-block:nth-child(3) {
      transition-delay: 0.4s;
    }
    .text-block:nth-child(4) {
      transition-delay: 0.5s;
    }
    .calendar-container {
      transition-delay: 0.7s;
    }
  }
}

/* Мобилки (до 768px) */
@media (max-width: 768px) {
  .invite-overlap {
    margin-top: -100px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .calendar-container {
    max-width: 500px;
    margin-left: 46px;
    display: flex;
    justify-content: center;
  }

  .text-block {
    transform: translateY(20px);
  }
}

/* Маленькие телефоны (до 480px) */
@media (max-width: 480px) {
  .invite-overlap {
    margin-top: -40px;
    padding-top: 60px;
    padding-bottom: 60px;
  }

  .calendar-container {
    max-width: 350px;
  }

  .text-block {
    transform: translateY(15px);
  }
}

/* Очень маленькие телефоны (до 375px) */
@media (max-width: 375px) {
  .invite-overlap {
    margin-top: -100px;
    padding-top: 50px;
    padding-bottom: 50px;
  }

  .calendar-container {
    max-width: 280px;
  }
}

/* Для пользователей с отключённой анимацией */
@media (prefers-reduced-motion: reduce) {
  .text-block,
  .calendar-container,
  .calendar-image {
    transition: none !important;
  }

  .invite-overlap.animate {
    .text-block,
    .calendar-container {
      opacity: 1;
      transform: none;
    }
  }
}
</style>
