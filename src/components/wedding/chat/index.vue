<template>
  <div
    ref="container"
    class="row items-center justify-center bg-image chat-overlap"
    :class="{ animate: isVisible }"
  >
    <div class="text-white text-center q-pa-xl">
      <div class="centered-content">
        <div class="text-h3 font-cormorant-sc q-mb-xl chat-title">ОБЩИЙ ЧАТ</div>

        <div
          class="font-cormorant-sc q-mb-lg chat-description"
          :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'"
        >
          <span v-if="!$q.screen.lt.md">
            Присоединяйтесь к нашему чату для гостей<br />
            в Telegram, чтобы знакомиться и<br />
            обмениваться фотографиями
          </span>
          <span v-else>
            Присоединяйтесь к нашему чату для гостей в Telegram, чтобы знакомиться и обмениваться
            фотографиями
          </span>
        </div>

        <div class="q-mb-xl chat-button-container">
          <q-btn
            rounded
            outline
            class="font-cormorant-sc chat-button"
            color="dark"
            text-color="white"
            label="Присоединиться"
            :size="!$q.screen.lt.md ? 'xl' : 'lg'"
            padding="20px 60px"
            @click="openChat"
          />
        </div>
      </div>

      <div class="left-aligned-section">
        <div
          class="font-cormorant-sc wait-text wait-animate"
          :class="!$q.screen.lt.md ? 'text-h2' : 'text-h3'"
        >
          ЖДЕМ ВАС!
        </div>
        <div
          class="font-cormorant-sc signature-text signature-animate"
          :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'"
        >
          с любовью Константин и Николь
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'

export default defineComponent({
  name: 'ChatComponent',
  setup() {
    const container = ref(null)
    const isVisible = ref(false)
    let observer = null

    const openChat = () => {
      const chatUrl = 'https://t.me/ваша_ссылка_на_чат'
      window.open(chatUrl, '_blank')
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
      openChat,
    }
  },
})
</script>

<style lang="scss" scoped>
.chat-overlap {
  position: relative;
  z-index: 10;
  margin-top: -100px;
  padding-top: 120px;
  padding-bottom: 120px;
}

.bg-image {
  background-image: url('/img/chat.png');
  background-repeat: no-repeat;
  background-position: center center;
  min-height: 1000px;
  display: flex;
  align-items: center;
  justify-content: center;

  /* Адаптивный подход */
  @media (min-width: 1920px) {
    background-size: 120% auto;
  }

  @media (min-width: 1440px) and (max-width: 1919px) {
    background-size: 120% auto;
  }

  @media (min-width: 1024px) and (max-width: 1439px) {
    background-size: 160% auto;
  }

  @media (min-width: 768px) and (max-width: 1023px) {
    background-size: 180% auto;
    min-height: 900px;
  }

  @media (max-width: 767px) {
    background-size: 230% auto;
    min-height: 800px;
  }

  @media (max-width: 480px) {
    background-size: 355% auto;
    min-height: 700px;
  }
}

.centered-content {
  max-width: 800px;
  height: 400px;
  margin: 0 auto;
}

.left-aligned-section {
  max-width: 800px;
  margin: 80px auto 0 auto;
  text-align: left;
}

.wait-text {
  margin-left: 20px;
  margin-bottom: 20px;
  opacity: 0;
  transform: translateX(-50px);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.signature-text {
  margin-left: 240px;
  margin-top: 40px;
  opacity: 0;
  transform: translateX(50px);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.chat-title {
  opacity: 0;
  transform: translateY(-30px) scale(0.95);
  transition: all 0.8s ease;
}

.chat-description {
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease 0.2s;
}

.chat-button {
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease 0.4s;
}

.q-btn {
  transition: all 0.3s ease;

  &:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
  }
}

@keyframes waitPulse {
  0%,
  100% {
    text-shadow: 0 0 5px rgba(255, 255, 255, 0.3);
  }
  50% {
    text-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
  }
}

.bg-image.animate {
  .chat-title {
    opacity: 1;
    transform: translateY(0) scale(1);
  }

  .chat-description {
    opacity: 1;
    transform: translateY(0);
  }

  .chat-button {
    opacity: 1;
    transform: translateY(0);
  }

  .wait-animate {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.6s;

    &:hover {
      animation: waitPulse 2s infinite;
    }
  }

  .signature-animate {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.8s;
  }
}

@media (max-width: 768px) {
  .chat-overlap {
    margin-top: -60px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .centered-content,
  .left-aligned-section {
    max-width: 600px;
  }

  .wait-text {
    margin-left: 5px;
  }

  .signature-text {
    margin-left: 40px;
  }

  .bg-image.animate {
    .chat-title {
      transition-delay: 0.1s;
    }

    .chat-description {
      transition-delay: 0.2s;
    }

    .chat-button {
      transition-delay: 0.3s;
    }

    .wait-animate {
      transition-delay: 0.4s;
    }

    .signature-animate {
      transition-delay: 0.5s;
    }
  }
}

@media (max-width: 480px) {
  .chat-overlap {
    margin-top: -40px;
    padding-top: 60px;
    padding-bottom: 60px;
  }

  .centered-content {
    height: auto;
    min-height: 300px;
  }

  .left-aligned-section {
    margin: 40px auto 0 auto;
  }

  .wait-text {
    margin-left: 0;
  }

  .signature-text {
    margin-left: 60px;
  }

  .bg-image.animate {
    .chat-title {
      transition-delay: 0.05s;
    }

    .chat-description {
      transition-delay: 0.1s;
    }

    .chat-button {
      transition-delay: 0.15s;
    }

    .wait-animate {
      transition-delay: 0.2s;
    }

    .signature-animate {
      transition-delay: 0.25s;
    }
  }
}

@media (max-width: 360px) {
  .signature-text {
    margin-left: 10px;
    font-size: 1rem;
  }

  .wait-text {
    font-size: 1.5rem;
  }
}
</style>
