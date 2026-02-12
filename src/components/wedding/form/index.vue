<template>
  <div
    ref="container"
    class="row items-center justify-center bg-white form"
    :class="{ animate: isVisible }"
  >
    <div class="text-dark text-center" :class="!$q.screen.lt.md ? 'q-pa-xl' : 'q-pa-md'">
      <div class="text-h3 font-cormorant-sc q-mb-xl form-title">АНКЕТА ГОСТЯ</div>

      <div
        class="font-cormorant-sc q-mb-xl form-description"
        :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'"
      >
        <span v-if="!$q.screen.lt.md">
          Пожалуйста подтвердите свое присутствие<br />
          до 1 июля 2026 года
        </span>
        <span v-else> Пожалуйста подтвердите свое присутствие до 1 июля 2026 года </span>
      </div>

      <q-btn
        rounded
        outline
        class="font-cormorant-sc form-button"
        color="dark"
        text-color="dark"
        label="Подтвердить"
        :size="!$q.screen.lt.md ? 'xl' : 'lg'"
        padding="20px 60px"
        @click="modalOpen = true"
      />
    </div>

    <!-- Модальное окно с формой -->
    <q-dialog v-model="modalOpen" full-width>
      <q-card style="max-width: 800px; width: 100%; height: 80vh">
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6">Форма подтверждения</div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-card-section style="height: calc(100% - 60px); padding: 0">
          <iframe
            src="https://docs.google.com/forms/d/e/1FAIpQLSetZTMyNnoCh2eVS7CyRMSyZnBOtG770ctuFxcc65W9K_UzWw/viewform?usp=publish-editor"
            width="100%"
            height="100%"
            frameborder="0"
            marginheight="0"
            marginwidth="0"
            style="border: none"
          >
            Загрузка…
          </iframe>
        </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'

export default defineComponent({
  name: 'FormComponent',
  setup() {
    const container = ref(null)
    const isVisible = ref(false)
    const modalOpen = ref(false)
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
          threshold: 0.3,
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
      modalOpen,
    }
  },
})
</script>

<style lang="scss" scoped>
.form {
  min-height: 600px;
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

.q-btn {
  transition: all 0.3s ease;

  &:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
  }
}

@keyframes buttonPulse {
  0% {
    box-shadow: 0 0 0 0 rgba(0, 0, 0, 0.2);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(0, 0, 0, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(0, 0, 0, 0);
  }
}

@keyframes textGlow {
  0%,
  100% {
    text-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  }
  50% {
    text-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
  }
}

.bg-white.animate {
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
      background-color: rgba(0, 0, 0, 0.05);
      animation: buttonPulse 1.5s infinite;
    }
  }
}

@media (max-width: 768px) {
  .bg-white.animate {
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
  .bg-white.animate {
    .form-title {
      transition-delay: 0.05s;
    }

    .form-description {
      transition-delay: 0.1s;
    }

    .form-button {
      transition-delay: 0.15s;
    }
  }
}

@media (max-width: 360px) {
  .form-title {
    font-size: 1.5rem;
  }

  .form-description {
    font-size: 1rem;
  }
}
</style>
