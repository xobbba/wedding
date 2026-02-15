<template>
  <div
    ref="container"
    class="chat-overlap"
    :class="{ animate: isVisible }"
  >
    <div class="bg-image"></div>
    <div class="content">
      <div class="text-white text-center q-pa-xl content-inner">
        <div class="centered-content">
          <div class="font-cormorant-sc q-mb-xl chat-title" :class="!$q.screen.lt.md ? 'text-h3' : 'text-h4'">ОБЩИЙ ЧАТ</div>

          <div
            class="font-cormorant-sc q-mb-lg chat-description"
            :class="!$q.screen.lt.md ? 'text-h4' : 'text-h6'"
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
              :size="!$q.screen.lt.md ? 'xl' : 'md'"
              padding="20px 60px"
              @click="openChat"
            />
          </div>
        </div>

        <div class="left-aligned-section">
          <div class="font-cormorant-sc wait-text" :class="!$q.screen.lt.md ? 'text-h2' : 'text-h4'">
            ЖДЕМ ВАС!
          </div>
          <div
            class="font-cormorant-sc signature-text"
            :class="!$q.screen.lt.md ? 'text-h4' : 'text-h6'"
          >
            с любовью Константин и Николь
          </div>
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
  z-index: 15;
  margin-top: -150px;
  padding-top: 200px;
  padding-bottom: 150px;
  overflow: hidden;
  width: 100%;
  pointer-events: none;

  .content {
    pointer-events: auto;
  }
}

.bg-image {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('/img/chat.png');
  background-repeat: no-repeat;
  background-position: center center;
  opacity: 1;
  transform: scale(1);
  will-change: transform, opacity;
  background-size: 110% auto;

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
    background-size: 260% auto;
  }

  @media (max-width: 1023px) {
    background-size: 395% auto;
  }

  @media (max-width: 400px) {
    background-size: 395% auto;
  }

  @media screen and (max-width: 1400px) and (min-height: 1020px) {
    background-size: 395% auto;
  }
}

.content {
  position: relative;
  z-index: 2;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.content-inner {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
}

.centered-content {
  max-width: 800px;
  height: 400px;
  margin: 0 auto;

  @media (max-width: 768px) {
    height: auto;
    min-height: 350px;
  }

  @media (max-width: 480px) {
    min-height: 300px;
  }
}

.left-aligned-section {
  max-width: 800px;
  margin: 80px auto 0 auto;
  text-align: left;

  @media (max-width: 768px) {
    max-width: 600px;
  }

  @media (max-width: 480px) {
    margin: 40px auto 0 auto;
  }
}

.wait-text {
  margin-left: 20px;
  margin-bottom: 20px;
  opacity: 0;
  transform: translateX(-50px);
  transition: opacity 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), transform 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
  will-change: transform, opacity;

  @media (max-width: 768px) {
    margin-left: 5px;
  }

  @media (max-width: 480px) {
    margin-left: 0;
  }
}

.signature-text {
  margin-left: 240px;
  margin-top: 40px;
  opacity: 0;
  transform: translateX(50px);
  transition: opacity 0.8s cubic-bezier(0.34, 1.56, 0.64, 1), transform 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
  will-change: transform, opacity;

  @media (max-width: 768px) {
    margin-left: 40px;
  }

  @media (max-width: 480px) {
    margin-left: 60px;
  }

  @media (max-width: 360px) {
    margin-left: 10px;
    font-size: 1rem;
  }
}

.chat-title {
  opacity: 0;
  transform: translateY(-30px) scale(0.95);
  transition: opacity 0.8s ease, transform 0.8s ease;
  will-change: transform, opacity;
}

.chat-description {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.8s ease, transform 0.8s ease;
  will-change: transform, opacity;
}

.chat-button {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.8s ease, transform 0.8s ease;
  will-change: transform, opacity;
}

.q-btn {
  transition: all 0.3s ease;

  &:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
  }
}

.chat-overlap.animate {
  .chat-title {
    opacity: 1;
    transform: translateY(0) scale(1);
    transition-delay: 0.2s;
  }

  .chat-description {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.4s;
  }

  .chat-button {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.6s;
  }

  .wait-text {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.8s;
  }

  .signature-text {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 1s;
  }
}

@media (max-width: 1024px) {
  .chat-overlap {
    margin-top: -120px;
    padding-top: 170px;
    padding-bottom: 120px;
    z-index: 15;
  }

  .chat-overlap.animate {
    .chat-title { transition-delay: 0.1s; }
    .chat-description { transition-delay: 0.2s; }
    .chat-button { transition-delay: 0.3s; }
    .wait-text { transition-delay: 0.4s; }
    .signature-text { transition-delay: 0.5s; }
  }
}

@media (max-width: 768px) {
  .chat-overlap {
    margin-top: -100px;
    padding-top: 150px;
    padding-bottom: 100px;
    z-index: 15;
  }

  .wait-text {
    transform: translateX(-30px);
  }

  .signature-text {
    transform: translateX(30px);
  }
}

@media (max-width: 480px) {
  .chat-overlap {
    margin-top: -80px;
    padding-top: 130px;
    padding-bottom: 80px;
    z-index: 15;
  }

  .chat-overlap.animate {
    .chat-title { transition-delay: 0.05s; }
    .chat-description { transition-delay: 0.1s; }
    .chat-button { transition-delay: 0.15s; }
    .wait-text { transition-delay: 0.2s; }
    .signature-text { transition-delay: 0.25s; }
  }

  .wait-text,
  .signature-text {
    transform: translateY(20px);
  }

  .chat-overlap.animate {
    .wait-text,
    .signature-text {
      transform: translateY(0);
    }
  }
}

@media (prefers-reduced-motion: reduce) {
  .bg-image,
  .chat-title,
  .chat-description,
  .chat-button,
  .wait-text,
  .signature-text {
    transition: none !important;
  }

  .chat-overlap.animate {
    .bg-image {
      opacity: 1;
    }

    .chat-title,
    .chat-description,
    .chat-button,
    .wait-text,
    .signature-text {
      opacity: 1;
      transform: none;
    }
  }
}
</style>
