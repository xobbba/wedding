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
        @click="openFormModal"
      />
    </div>

    <!-- Модальное окно с формой -->
    <q-dialog v-model="modalOpen" full-width position="bottom">
      <q-card
        class="form-modal"
        :style="
          $q.screen.lt.md ? 'max-width: 100%; height: 90vh;' : 'max-width: 800px; height: 85vh;'
        "
      >
        <q-card-section class="row items-center q-pb-none bg-white">
          <div class="text-h6 font-cormorant-sc">ПОДТВЕРЖДЕНИЕ ПРИСУТСТВИЯ</div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup class="close-btn" />
        </q-card-section>

        <q-card-section class="form-container">
          <!-- Вариант 1: Google Form с кастомными стилями через CSS -->
          <div v-show="!useGoogleForms" class="custom-form">
            <form @submit.prevent="submitForm">
              <div class="form-group">
                <label class="font-cormorant-sc">Ваше имя и фамилия</label>
                <q-input
                  v-model="formData.name"
                  outlined
                  dense
                  placeholder="Иван Иванов"
                  class="q-mb-md"
                  :rules="[(val) => !!val || 'Поле обязательно']"
                />
              </div>

              <div class="form-group">
                <label class="font-cormorant-sc">Будете ли вы присутствовать?</label>
                <q-option-group
                  v-model="formData.attendance"
                  :options="attendanceOptions"
                  color="dark"
                  inline
                  class="q-mb-md"
                />
              </div>

              <div class="form-group">
                <label class="font-cormorant-sc">Количество гостей</label>
                <q-select
                  v-model="formData.guests"
                  :options="[1, 2, 3, 4, 5]"
                  outlined
                  dense
                  placeholder="Выберите количество"
                  class="q-mb-md"
                />
              </div>

              <div class="form-group">
                <label class="font-cormorant-sc">Пожелания по питанию</label>
                <q-input
                  v-model="formData.dietary"
                  outlined
                  dense
                  placeholder="Аллергии, предпочтения и т.д."
                  class="q-mb-md"
                />
              </div>

              <div class="form-group">
                <label class="font-cormorant-sc">Ваш Telegram для чата</label>
                <q-input
                  v-model="formData.telegram"
                  outlined
                  dense
                  placeholder="@username"
                  class="q-mb-lg"
                />
              </div>

              <q-btn
                rounded
                unelevated
                color="dark"
                label="Отправить"
                type="submit"
                size="lg"
                class="full-width submit-btn font-cormorant-sc"
              />
            </form>
          </div>

          <!-- Вариант 2: Google Form с кастомизацией через URL параметры -->
          <div v-show="useGoogleForms" class="google-form-wrapper">
            <iframe
              :src="googleFormUrl"
              width="100%"
              height="100%"
              frameborder="0"
              marginheight="0"
              marginwidth="0"
              style="border: none; min-height: 500px"
              :title="googleFormTitle"
              referrerpolicy="no-referrer-when-downgrade"
              @load="onIframeLoad"
            >
              Загрузка формы...
            </iframe>
          </div>
        </q-card-section>

        <q-card-section
          v-if="useGoogleForms"
          class="text-center text-caption text-grey-7 q-pt-none"
        >
          Форма защищена Google. Ваши ответы будут отправлены организаторам.
        </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'FormComponent',
  setup() {
    const $q = useQuasar()
    const container = ref(null)
    const isVisible = ref(false)
    const modalOpen = ref(false)
    const useGoogleForms = ref(true) // Переключи на false для кастомной формы
    let observer = null

    const formData = ref({
      name: '',
      attendance: 'yes',
      guests: 1,
      dietary: '',
      telegram: '',
    })

    const attendanceOptions = [
      { label: 'Буду с удовольствием', value: 'yes' },
      { label: 'Не смогу присутствовать', value: 'no' },
      { label: 'Только на церемонию', value: 'ceremony' },
    ]

    // Google Form URL с параметрами для кастомизации
    const googleFormUrl = ref('')
    const googleFormTitle = 'RSVP Form'

    const generateGoogleFormUrl = () => {
      const baseUrl =
        'https://docs.google.com/forms/d/e/1FAIpQLSetZTMyNnoCh2eVS7CyRMSyZnBOtG770ctuFxcc65W9K_UzWw/viewform'
      const params = new URLSearchParams({
        usp: 'pp_url',
        embedded: 'true',
        // Параметры для скрытия элементов Google Forms
        hl: 'ru',
        theme: '0',
        bgcolor: '#FFFFFF',
        fvv: '1',
        partial: 'false',
      })

      return `${baseUrl}?${params.toString()}`
    }

    const openFormModal = () => {
      modalOpen.value = true
      if (useGoogleForms.value) {
        googleFormUrl.value = generateGoogleFormUrl()
      }
    }

    const onIframeLoad = () => {
      // Попытка инжектить CSS в iframe (работает только если форма на том же домене)
      try {
        const iframe = document.querySelector('iframe')
        if (iframe && iframe.contentDocument) {
          const style = iframe.contentDocument.createElement('style')
          style.textContent = `
            body {
              font-family: 'Cormorant SC', serif !important;
            }
            .ss-form-heading, .freebirdFormviewerViewHeaderHeader,
            .freebirdFormviewerViewFooterFooter, .freebirdHeader {
              display: none !important;
            }
            .freebirdFormviewerViewItemsItemItem {
              background: #fafafa !important;
              border-radius: 8px !important;
            }
          `
          iframe.contentDocument.head.appendChild(style)
        }
      } catch (e) {
        console.log(e)
      }
    }

    const submitForm = () => {
      // Здесь логика отправки кастомной формы
      console.log('Form submitted:', formData.value)

      $q.notify({
        type: 'positive',
        message: 'Спасибо! Ваш ответ отправлен.',
        position: 'top',
        timeout: 3000,
      })

      modalOpen.value = false

      // Очистка формы
      formData.value = {
        name: '',
        attendance: 'yes',
        guests: 1,
        dietary: '',
        telegram: '',
      }
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
          threshold: 0.3,
          rootMargin: '50px',
        },
      )

      if (container.value) {
        observer.observe(container.value)
      }

      googleFormUrl.value = generateGoogleFormUrl()
    })

    onUnmounted(() => {
      if (observer) observer.disconnect()
    })

    return {
      container,
      isVisible,
      modalOpen,
      useGoogleForms,
      formData,
      attendanceOptions,
      googleFormUrl,
      googleFormTitle,
      openFormModal,
      onIframeLoad,
      submitForm,
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

.form-modal {
  border-radius: 20px 20px 0 0;
  overflow: hidden;

  @media (min-width: 768px) {
    border-radius: 20px;
  }
}

.close-btn {
  transition: transform 0.3s ease;

  &:hover {
    transform: rotate(90deg);
  }
}

.form-container {
  height: calc(100% - 70px);
  padding: 24px;
  overflow-y: auto;
  background: linear-gradient(135deg, #f5f7fa 0%, #ffffff 100%);
}

.custom-form {
  max-width: 600px;
  margin: 0 auto;

  .form-group {
    margin-bottom: 20px;

    label {
      display: block;
      margin-bottom: 8px;
      font-size: 1.1rem;
      font-weight: 500;
      color: #2c3e50;
    }
  }

  .submit-btn {
    height: 56px;
    font-size: 1.2rem;
    margin-top: 20px;
    transition: all 0.3s ease;

    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
    }
  }
}

.google-form-wrapper {
  width: 100%;
  height: 100%;
  background: white;
  border-radius: 8px;
  overflow: hidden;

  iframe {
    width: 100%;
    height: 100%;
    min-height: 600px;
    background: white;
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
  .form-container {
    padding: 16px;
  }

  .custom-form {
    .submit-btn {
      height: 48px;
      font-size: 1rem;
    }
  }

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
  .form-container {
    padding: 12px;
  }

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

/* Стилизация скроллбара */
.form-container::-webkit-scrollbar {
  width: 6px;
}

.form-container::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}

.form-container::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 10px;
}

.form-container::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>
