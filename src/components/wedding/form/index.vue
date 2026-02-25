<template>
  <div
    ref="container"
    class="row items-center justify-center bg-white form"
    :class="{ animate: isVisible }"
  >
    <div
      class="text-dark text-center form-content"
      :class="!$q.screen.lt.md ? 'q-pa-xl' : 'q-pa-md'"
      style="margin-top: -120px"
    >
      <div
        class="font-cormorant-sc q-mb-xl form-title"
        :class="!$q.screen.lt.md ? 'text-h3' : 'text-h4'"
      >
        АНКЕТА ГОСТЯ
      </div>

      <div
        class="font-cormorant-sc q-mb-xl form-description"
        :class="!$q.screen.lt.md ? 'text-h4' : 'text-h6'"
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
        label="Заполнить анкету"
        :size="!$q.screen.lt.md ? 'xl' : 'md'"
        padding="20px 60px"
        @click="modalOpen = true"
      />
    </div>

    <q-dialog v-model="modalOpen" persistent>
      <q-card class="form-modal">
        <q-card-section class="row items-center q-pb-none bg-white">
          <div class="font-cormorant-sc" :class="!$q.screen.lt.md ? 'text-h5' : 'text-h6'">
            ПОДТВЕРЖДЕНИЕ
          </div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup class="close-btn" />
        </q-card-section>

        <q-card-section class="form-container">
          <div class="custom-form">
            <form @submit.prevent="submitToGoogleSheet">
              <!-- 1. ФИО -->
              <div class="form-group">
                <label class="font-cormorant-sc">1. Ваше ФИО *</label>
                <q-input
                  v-model="formData.name"
                  outlined
                  dense
                  placeholder="Иванов Иван Иванович"
                  class="q-mb-md"
                  :error="!!errors.name"
                  :error-message="errors.name"
                  required
                />
              </div>

              <!-- 2. Присутствие -->
              <div class="form-group">
                <label class="font-cormorant-sc">2. Сможете ли Вы присутствовать? *</label>
                <q-option-group
                  v-model="formData.attendance"
                  :options="attendanceOptions"
                  color="dark"
                  inline
                  class="q-mb-md"
                />
                <div v-if="errors.attendance" class="text-negative text-caption q-mt-xs">
                  {{ errors.attendance }}
                </div>
              </div>

              <!-- 3. Семья с подсказкой -->
              <div class="form-group">
                <label class="font-cormorant-sc">
                  3. Если Вы придете с семьей, внесите все имена, а также возраст детей
                  <q-icon
                    name="info"
                    size="18px"
                    class="q-ml-xs text-grey-6 cursor-help"
                    style="vertical-align: middle"
                  >
                    <q-tooltip
                      anchor="center right"
                      self="center left"
                      :offset="[5, 5]"
                      class="bg-dark text-white tooltip-text"
                      style="
                        font-size: 14px;
                        width: 320px;
                        white-space: normal;
                        line-height: 1.3;
                        word-break: break-word;
                      "
                    >
                      Обращаем ваше внимание, что мероприятие предназначено исключительно для
                      взрослых гостей - детский стол и аниматоры не предусмотрены
                    </q-tooltip>
                  </q-icon>
                </label>
                <q-input
                  v-model="formData.familyMembers"
                  outlined
                  dense
                  type="textarea"
                  placeholder="ФИО (возраст)"
                  class="q-mb-md"
                  autogrow
                />
              </div>

              <!-- 4. Напитки -->
              <div class="form-group">
                <label class="font-cormorant-sc"
                  >4. Что будете пить? (можно выбрать несколько)</label
                >

                <div class="drink-option">
                  <q-checkbox
                    v-model="formData.drinks.champagne"
                    label="Шампанское"
                    color="dark"
                    dense
                  />
                </div>

                <div class="drink-option">
                  <q-checkbox
                    v-model="formData.drinks.whiteDry"
                    label="Белое вино сухое"
                    color="dark"
                    dense
                  />
                </div>

                <div class="drink-option">
                  <q-checkbox
                    v-model="formData.drinks.whiteSemiSweet"
                    label="Белое вино полусладкое"
                    color="dark"
                    dense
                  />
                </div>

                <div class="drink-option">
                  <q-checkbox
                    v-model="formData.drinks.redSemiSweet"
                    label="Красное вино полусладкое"
                    color="dark"
                    dense
                  />
                </div>

                <div class="drink-option">
                  <q-checkbox
                    v-model="formData.drinks.redDry"
                    label="Красное вино сухое"
                    color="dark"
                    dense
                  />
                </div>

                <div class="drink-option">
                  <q-checkbox v-model="formData.drinks.whiskey" label="Виски" color="dark" dense />
                </div>

                <div class="drink-option">
                  <q-checkbox v-model="formData.drinks.vodka" label="Водка" color="dark" dense />
                </div>

                <div class="drink-option">
                  <q-checkbox v-model="formData.drinks.cognac" label="Коньяк" color="dark" dense />
                </div>

                <div class="drink-option">
                  <q-checkbox v-model="formData.drinks.other" label="Другое" color="dark" dense />
                  <q-input
                    v-if="formData.drinks.other"
                    v-model="formData.drinks.otherText"
                    outlined
                    dense
                    placeholder="Укажите ваш вариант"
                    class="q-mt-sm q-mb-md"
                  />
                </div>
              </div>

              <!-- 5. Аллергии -->
              <div class="form-group">
                <label class="font-cormorant-sc"
                  >5. Есть ли у вас аллергия на какие-то продукты?</label
                >
                <q-option-group
                  v-model="formData.allergy.hasAllergy"
                  :options="allergyOptions"
                  color="dark"
                  inline
                  class="q-mb-md"
                />
                <q-input
                  v-if="formData.allergy.hasAllergy === 'yes'"
                  v-model="formData.allergy.allergyDetails"
                  outlined
                  dense
                  placeholder="Укажите на какие продукты у вас аллергия"
                  class="q-mt-sm"
                />
              </div>

              <!-- 6. Любимый трек -->
              <div class="form-group">
                <label class="font-cormorant-sc">6. Оставьте свой любимый музыкальный трек</label>
                <q-input
                  v-model="formData.favoriteTrack"
                  outlined
                  dense
                  placeholder="Исполнитель - Название трека"
                  class="q-mb-md"
                />
              </div>

              <!-- 7. Комментарий -->
              <div class="form-group">
                <label class="font-cormorant-sc">7. Комментарий</label>
                <q-input
                  v-model="formData.comment"
                  outlined
                  dense
                  type="textarea"
                  placeholder="Ваши пожелания, вопросы или особые просьбы..."
                  class="q-mb-md"
                  autogrow
                />
              </div>

              <q-btn
                rounded
                unelevated
                color="dark"
                :label="submitting ? 'Отправка...' : 'Отправить ответ'"
                type="submit"
                size="lg"
                class="full-width submit-btn font-cormorant-sc"
                :loading="submitting"
                :disable="submitting"
              />
            </form>
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted, reactive } from 'vue'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'FormComponent',
  setup() {
    const $q = useQuasar()
    const container = ref(null)
    const isVisible = ref(false)
    const modalOpen = ref(false)
    const submitting = ref(false)
    let observer = null

    const formData = reactive({
      name: '',
      attendance: null,
      familyMembers: '',
      drinks: {
        champagne: false,
        whiteDry: false,
        whiteSemiSweet: false,
        redSemiSweet: false,
        redDry: false,
        whiskey: false,
        vodka: false,
        cognac: false,
        other: false,
        otherText: '',
      },
      allergy: {
        hasAllergy: null,
        allergyDetails: '',
      },
      favoriteTrack: '',
      comment: '',
    })

    const errors = ref({
      name: '',
      attendance: '',
    })

    const attendanceOptions = [
      { label: 'Да', value: 'yes' },
      { label: 'Нет', value: 'no' },
    ]

    const allergyOptions = [
      { label: 'Да', value: 'yes' },
      { label: 'Нет', value: 'no' },
    ]

    const formatDrinks = () => {
      const selected = []

      if (formData.drinks.champagne) selected.push('Шампанское')
      if (formData.drinks.whiteDry) selected.push('Белое вино сухое')
      if (formData.drinks.whiteSemiSweet) selected.push('Белое вино полусладкое')
      if (formData.drinks.redSemiSweet) selected.push('Красное вино полусладкое')
      if (formData.drinks.redDry) selected.push('Красное вино сухое')
      if (formData.drinks.whiskey) selected.push('Виски')
      if (formData.drinks.vodka) selected.push('Водка')
      if (formData.drinks.cognac) selected.push('Коньяк')

      if (formData.drinks.other) {
        if (formData.drinks.otherText.trim()) {
          selected.push(`Другое: ${formData.drinks.otherText.trim()}`)
        } else {
          selected.push('Другое')
        }
      }

      return selected.length > 0 ? selected.join(', ') : 'Не выбрано'
    }

    const submitToGoogleSheet = async () => {
      errors.value.name = ''
      errors.value.attendance = ''

      if (!formData.name.trim()) {
        errors.value.name = 'Пожалуйста, укажите ваше ФИО'
        $q.notify({
          type: 'negative',
          message: 'Пожалуйста, укажите ваше ФИО',
          position: 'top-right',
          timeout: 3000,
        })
        return
      }

      if (!formData.attendance) {
        errors.value.attendance = 'Пожалуйста, укажите, будете ли вы на свадьбе'
        $q.notify({
          type: 'warning',
          message: 'Выберите, будете ли вы на свадьбе',
          position: 'top-right',
          timeout: 3000,
        })
        return
      }

      submitting.value = true

      const loadingNotify = $q.notify({
        type: 'info',
        message: 'Отправка ответа...',
        position: 'top-right',
        timeout: 0,
        group: false,
      })

      try {
        const scriptURL =
          'https://script.google.com/macros/s/AKfycbx6LJXexp7HEO4gmCVj2Rk5VYpARCXWNIcql8zmc7Kxj_1R-eDHO8ZnCyPkTgjeJgV-/exec'

        const formDataToSend = new FormData()
        formDataToSend.append('timestamp', new Date().toLocaleString('ru-RU'))
        formDataToSend.append('name', formData.name.trim())
        formDataToSend.append('attendance', formData.attendance === 'yes' ? 'Да' : 'Нет')
        formDataToSend.append('familyMembers', formData.familyMembers.trim() || '-')
        formDataToSend.append('drinks', formatDrinks())
        formDataToSend.append(
          'allergy',
          formData.allergy.hasAllergy === 'yes'
            ? `Да: ${formData.allergy.allergyDetails || 'не указано'}`
            : 'Нет',
        )
        formDataToSend.append('favoriteTrack', formData.favoriteTrack.trim() || '-')
        formDataToSend.append('comment', formData.comment.trim() || '-')

        await fetch(scriptURL, {
          method: 'POST',
          body: formDataToSend,
          mode: 'no-cors',
        })

        loadingNotify()

        $q.notify({
          type: 'positive',
          message: 'Спасибо! Ваш ответ отправлен.',
          position: 'top-right',
          timeout: 5000,
        })

        modalOpen.value = false

        formData.name = ''
        formData.attendance = null
        formData.familyMembers = ''
        formData.drinks = {
          champagne: false,
          whiteDry: false,
          whiteSemiSweet: false,
          redSemiSweet: false,
          redDry: false,
          whiskey: false,
          vodka: false,
          cognac: false,
          other: false,
          otherText: '',
        }
        formData.allergy = {
          hasAllergy: null,
          allergyDetails: '',
        }
        formData.favoriteTrack = ''
        formData.comment = ''
      } catch (error) {
        console.error('Error:', error)

        loadingNotify()

        $q.notify({
          type: 'negative',
          message: 'Ошибка при отправке. Попробуйте еще раз.',
          position: 'top-right',
          timeout: 5000,
        })
      } finally {
        submitting.value = false
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
    })

    onUnmounted(() => {
      if (observer) observer.disconnect()
    })

    return {
      container,
      isVisible,
      modalOpen,
      submitting,
      formData,
      errors,
      attendanceOptions,
      allergyOptions,
      submitToGoogleSheet,
    }
  },
})
</script>

<style lang="scss" scoped>
.form {
  min-height: 700px;
  background: white;
}

.form-content {
  max-width: 800px;
  margin: 0 auto;
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
  max-width: 600px;
  width: 100%;
  border-radius: 20px;
  overflow: hidden;

  @media (max-width: 480px) {
    max-width: 95%;
    margin: 0 auto;
  }
}

.close-btn {
  transition: transform 0.3s ease;

  &:hover {
    transform: rotate(90deg);
  }
}

.form-container {
  max-height: 80vh;
  padding: 24px;
  overflow-y: auto;
  background: linear-gradient(135deg, #f9f9f9 0%, #ffffff 100%);
}

.custom-form {
  max-width: 500px;
  margin: 0 auto;

  .form-group {
    margin-bottom: 28px;

    label {
      display: block;
      margin-bottom: 12px;
      font-size: 1.1rem;
      font-weight: 500;
      color: #2c3e50;
      letter-spacing: 0.5px;
    }
  }

  .drink-option {
    margin-bottom: 8px;

    .q-checkbox {
      margin-bottom: 4px;
    }
  }

  .submit-btn {
    height: 56px;
    font-size: 1.2rem;
    margin-top: 20px;
    transition: all 0.3s ease;
    background: #1a1a1a;

    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      background: #000;
    }
  }
}

.text-negative {
  color: #c10015;
  font-size: 0.85rem;
  margin-top: 4px;
}

.tooltip-text {
  white-space: normal;
  line-height: 1.4;
  padding: 10px;
  border-radius: 8px;
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
    .form-group {
      margin-bottom: 24px;

      label {
        font-size: 1rem;
      }
    }

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

:deep(.q-field__control) {
  border-radius: 8px;
}

:deep(.q-field__control:hover) {
  border-color: #000;
}

:deep(.q-checkbox__label) {
  font-size: 1rem;
}

:deep(.q-radio__label) {
  font-family: 'Cormorant SC', serif;
  font-size: 1rem;
}
</style>
