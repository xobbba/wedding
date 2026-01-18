<template>
  <div
    ref="container"
    class="row items-center justify-center bg-image"
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
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'InviteComponent',
  setup() {
    const $q = useQuasar()
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
      $q,
      container,
      isVisible
    }
  }
})
</script>

<style lang="scss" scoped>
.bg-image {
  background-image: url('/img/invite.png');
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
  min-height: 1200px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  animation: fadeIn 0.8s ease-out forwards;
}

// Анимации
@keyframes fadeIn {
  to {
    opacity: 1;
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

// Блоки текста
.text-block {
  opacity: 0;
  transform: translateY(20px);
}

.bg-image.animate {
  .text-block:nth-child(1) {
    animation: textAppear 0.8s ease 0.2s forwards;
  }

  .text-block:nth-child(2) {
    animation: textAppear 0.8s ease 0.4s forwards;
  }

  .text-block:nth-child(3) {
    animation: textAppear 0.8s ease 0.6s forwards;
  }

  .text-block:nth-child(4) {
    animation: textAppear 0.8s ease 0.8s forwards;
  }

  .calendar-image {
    opacity: 0;
    animation: calendarAppear 1s ease 1s forwards;
  }
}

.calendar-container {
  max-width: 800px;
  margin: 0 auto;
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

// Адаптивность
@media (max-width: 1024px) {
  .bg-image {
    min-height: 1000px;
    padding: 40px 20px;
  }

  .bg-image.animate {
    .text-block:nth-child(1) { animation-delay: 0.1s !important; }
    .text-block:nth-child(2) { animation-delay: 0.2s !important; }
    .text-block:nth-child(3) { animation-delay: 0.3s !important; }
    .text-block:nth-child(4) { animation-delay: 0.4s !important; }
    .calendar-image { animation-delay: 0.5s !important; }
  }
}

@media (max-width: 768px) {
  .bg-image {
    min-height: 800px;
    padding: 30px 15px;
  }

  .calendar-container {
    max-width: 500px;
  }
}

@media (max-width: 480px) {
  .bg-image {
    min-height: 700px;
  }

  .calendar-container {
    max-width: 350px;
  }
}
</style>
