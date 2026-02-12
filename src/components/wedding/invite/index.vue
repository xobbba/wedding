<template>
  <div
    ref="container"
    class="row items-center justify-center bg-image invite-overlap"
    :class="{ 'animate': isVisible }"
  >
    <div class="text-white text-center q-pa-xl">
      <div class="text-h3 font-cormorant-sc q-mb-lg text-block">
        <span v-if="!$q.screen.lt.md">
          ДОРОГИЕ РОДНЫЕ<br>И БЛИЗКИЕ!
        </span>
        <span v-else>
          ДОРОГИЕ РОДНЫЕ И БЛИЗКИЕ!
        </span>
      </div>

      <div
        class="font-cormorant-sc q-mb-md text-block"
        :class="[!$q.screen.lt.md ? 'text-h4' : 'text-h5']"
      >
        <span v-if="!$q.screen.lt.md">
          Мы давно ждали этого момента, когда сможем<br>разделить с вами этот важный и счастливый<br>день в нашей жизни.
        </span>
        <span v-else>
          Мы давно ждали этого момента, когда сможем разделить с вами этот важный и счастливый день в нашей жизни.
        </span>
      </div>

      <div
        class="font-cormorant-sc q-mb-lg text-block"
        :class="[!$q.screen.lt.md ? 'text-h4' : 'text-h5']"
      >
        <span v-if="!$q.screen.lt.md">
          Совсем скоро состоится<br>наша свадьба!
        </span>
        <span v-else>
          Совсем скоро состоится наша свадьба!
        </span>
      </div>

      <div
        class="font-cormorant-sc q-mb-xl text-block"
        :class="[!$q.screen.lt.md ? 'text-h4' : 'text-h5']"
      >
        <span v-if="!$q.screen.lt.md">
          Мы рады пригласить вас стать<br>свидетелями этого торжества и разделить с нами<br>самые яркие моменты
        </span>
        <span v-else>
          Мы рады пригласить вас стать свидетелями этого торжества и разделить с нами самые яркие моменты
        </span>
      </div>

      <div class="calendar-container">
        <img src="/img/calendar.png" alt="Календарь" class="calendar-image">
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
      observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            isVisible.value = true
            observer.disconnect()
          }
        })
      }, {
        threshold: 0.1,
        rootMargin: '50px'
      })

      if (container.value) {
        observer.observe(container.value)
      }
    })

    onUnmounted(() => {
      if (observer) observer.disconnect()
    })

    return {
      container,
      isVisible
    }
  }
})
</script>

<style lang="scss" scoped>
.invite-overlap {
  position: relative;
  z-index: 6;
  margin-top: -100px;
  padding-top: 120px;
  padding-bottom: 120px;
}

.bg-image {
  background-image: url('/img/invite.png');
  background-repeat: no-repeat;
  background-position: center center;
  min-height: 1400px;
  display: flex;
  align-items: center;
  justify-content: center;

  /* Вариант 4: Адаптивный подход с разными значениями */
  @media (min-width: 1920px) {
    background-size: 120% auto;
  }

  @media (min-width: 1440px) and (max-width: 1919px) {
    background-size: 140% auto;
  }

  @media (min-width: 1024px) and (max-width: 1439px) {
    background-size: 184% auto;
  }

  @media (min-width: 768px) and (max-width: 1023px) {
    background-size: 210% auto;
    min-height: 1200px;
  }

  @media (max-width: 767px) {
    background-size: 250% auto;
    min-height: 1000px;
  }

  @media (max-width: 480px) {
    background-size: 444% auto;
    min-height: 800px;
  }
}

.text-block {
  opacity: 0;
  transform: translateY(20px);
}

.calendar-container {
  max-width: 700px;
  margin-left: 40px;
  opacity: 0;
  transform: translateY(20px);
}

.calendar-image {
  max-width: 100%;
  width: 100%;
  height: auto;
  transition: transform 0.3s ease;

  &:hover {
    transform: scale(1.01);
  }
}

.bg-image.animate {
  .text-block:nth-child(1) {
    opacity: 1;
    transform: translateY(0);
    animation: textAppear 0.8s ease 0.2s forwards;
  }

  .text-block:nth-child(2) {
    opacity: 1;
    transform: translateY(0);
    animation: textAppear 0.8s ease 0.4s forwards;
  }

  .text-block:nth-child(3) {
    opacity: 1;
    transform: translateY(0);
    animation: textAppear 0.8s ease 0.6s forwards;
  }

  .text-block:nth-child(4) {
    opacity: 1;
    transform: translateY(0);
    animation: textAppear 0.8s ease 0.8s forwards;
  }

  .calendar-container {
    opacity: 1;
    transform: translateY(0);
    animation: calendarAppear 1s ease 1s forwards;
  }
}

@keyframes textAppear {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes calendarAppear {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 1024px) {
  .invite-overlap {
    margin-top: -80px;
    padding-top: 100px;
    padding-bottom: 100px;
  }

  .bg-image.animate {
    .text-block:nth-child(1) { animation-delay: 0.1s; }
    .text-block:nth-child(2) { animation-delay: 0.2s; }
    .text-block:nth-child(3) { animation-delay: 0.3s; }
    .text-block:nth-child(4) { animation-delay: 0.4s; }
    .calendar-container { animation-delay: 0.5s; }
  }
}

@media (max-width: 768px) {
  .invite-overlap {
    margin-top: -60px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .calendar-container {
    max-width: 500px;
    margin-left: 0;
  }
}

@media (max-width: 480px) {
  .invite-overlap {
    margin-top: -40px;
    padding-top: 60px;
    padding-bottom: 60px;
  }

  .calendar-container {
    max-width: 350px;
  }
}
</style>
