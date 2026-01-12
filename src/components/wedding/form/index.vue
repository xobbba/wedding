<template>
  <div
    ref="container"
    class="row items-center justify-center bg-image"
    :class="{ 'animate': isVisible }"
  >
    <div class="text-white text-center" :class="$q.screen.md ? 'q-pa-xl' : 'q-pa-md'">
      <div
        class="font-cormorant-sc q-mb-xl form-title"
        :class="$q.screen.md ? 'text-h2' : 'text-h3'"
      >
        АНКЕТА ГОСТЯ
      </div>

      <div
        class="font-cormorant-sc q-mb-xl form-description"
        :class="$q.screen.md ? 'text-h4' : 'text-h5'"
      >
        Пожалуйста подтвердите свое присутствие<br>
        до 1 июля 2026 года
      </div>

      <q-btn
        rounded
        outline
        class="font-cormorant-sc form-button"
        color="white"
        text-color="white"
        label="Подтвердить"
        :size="$q.screen.md ? 'xl' : 'lg'"
        padding="20px 60px"
        @click="openForm"
      />
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'

export default defineComponent({
  name: 'FormComponent',
  setup() {
    const container = ref(null)
    const isVisible = ref(false)
    let observer = null

    const openForm = () => {
      const formUrl = 'https://forms.yandex.ru/ваша_ссылка_на_форму'
      window.open(formUrl, '_blank')
    }

    onMounted(() => {
      observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            isVisible.value = true
            if (observer) {
              observer.disconnect()
              observer = null
            }
          }
        })
      }, {
        threshold: 0.3,
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
      isVisible,
      openForm
    }
  }
})
</script>

<style lang="scss" scoped>
.bg-image {
  background-image: url('/img/form.png');
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
  min-height: 800px;
  width: 100%;
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s ease-out;
}

.form-title,
.form-description,
.form-button {
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.6s ease-out;
}

.form-title {
  transform: translateY(-30px);
  transition-delay: 0.2s;
}

.form-description {
  transform: translateY(30px);
  transition-delay: 0.4s;
}

.form-button {
  transform: scale(0.8);
  transition: all 0.8s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  transition-delay: 0.6s;
}

@keyframes buttonPulse {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.4);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(255, 255, 255, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0);
  }
}

@keyframes textGlow {
  0%, 100% {
    text-shadow: 0 0 5px rgba(255, 255, 255, 0.3);
  }
  50% {
    text-shadow: 0 0 15px rgba(255, 255, 255, 0.6);
  }
}

.bg-image.animate {
  opacity: 1;
  transform: translateY(0);

  .form-title {
    opacity: 1;
    transform: translateY(0);

    &:hover {
      animation: textGlow 2s infinite;
    }
  }

  .form-description {
    opacity: 1;
    transform: translateY(0);
  }

  .form-button {
    opacity: 1;
    transform: scale(1);

    &:hover {
      transform: scale(1.05);
      background-color: rgba(255, 255, 255, 0.1);
      animation: buttonPulse 1.5s infinite;
    }
  }
}

@media (max-width: 768px) {
  .bg-image {
    min-height: 600px;
  }

  .bg-image.animate {
    .form-title {
      transition-delay: 0.1s;
    }

    .form-description {
      transition-delay: 0.2s;
    }

    .form-button {
      transition-delay: 0.3s;
    }
  }
}

@media (max-width: 480px) {
  .bg-image {
    min-height: 500px;
  }
}
</style>
