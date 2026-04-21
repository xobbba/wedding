<template>
  <div
    ref="container"
    class="row items-center justify-center bg-white form"
    :class="{ animate: isVisible }"
  >
    <div
      class="text-dark text-center form-content"
      :class="!$q.screen.lt.sm ? 'q-pa-xl' : 'q-pa-md'"
      style="margin-top: -120px"
    >
      <div
        class="font-cormorant-sc q-mb-xl form-title"
        :class="!$q.screen.lt.sm ? 'text-h3' : 'text-h4'"
      >
        АНКЕТА ГОСТЯ
      </div>

      <div
        class="font-cormorant-sc q-mb-xl form-description"
        :class="!$q.screen.lt.sm ? 'text-h4' : 'text-h6'"
      >
        <span v-if="!$q.screen.lt.sm">
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
        :size="!$q.screen.lt.sm ? 'xl' : 'md'"
        padding="20px 60px"
        @click="modalOpen = true"
      />
    </div>

    <q-dialog v-model="modalOpen" persistent>
      <q-card class="form-modal">
        <q-card-section class="row items-center q-pb-none bg-white">
          <div class="font-cormorant-sc" :class="!$q.screen.lt.sm ? 'text-h5' : 'text-h6'">
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

              <!-- 3. Семья с подсказкой (исправлено для мобильных) -->
              <div class="form-group">
                <label class="font-cormorant-sc">
                  3. Если Вы придете с семьей, внесите все имена, а также возраст детей
                  <q-icon
                    name="info"
                    size="18px"
                    class="q-ml-xs text-grey-6 cursor-help"
                    style="vertical-align: middle"
                    @click="showTooltipDialog = true"
                  >
                    <q-tooltip
                      v-if="!isMobile"
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
                  >4. Предпочтения по напиткам (можно выбрать несколько)</label
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
                    v-model="formData.drinks.whiteWine"
                    label="Белое вино"
                    color="dark"
                    dense
                  />
                </div>

                <div class="drink-option">
                  <q-checkbox
                    v-model="formData.drinks.redWine"
                    label="Красное вино"
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

    <q-dialog v-model="showTooltipDialog">
      <q-card class="bg-dark text-white" style="max-width: 90vw; border-radius: 12px">
        <q-card-section class="row items-center justify-between">
          <div class="text-h6">Внимание</div>
          <q-btn v-close-popup flat round dense icon="close" color="white" />
        </q-card-section>
        <q-card-section style="font-size: 16px; line-height: 1.4">
          Обращаем ваше внимание, что мероприятие предназначено исключительно для взрослых гостей -
          детский стол и аниматоры не предусмотрены
        </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted, reactive } from 'vue'
import { useQuasar } from 'quasar'

const RSVP_STORAGE_KEY = 'wedding-rsvp-submissions'
const MAX_STORED_SUBMISSIONS = 20

export default defineComponent({
  name: 'FormComponent',
  setup() {
    const $q = useQuasar()
    const container = ref(null)
    const isVisible = ref(false)
    const modalOpen = ref(false)
    const submitting = ref(false)
    const showTooltipDialog = ref(false)
    const isMobile = ref(false)
    let observer = null

    const formData = reactive({
      name: '',
      attendance: null,
      familyMembers: '',
      drinks: {
        champagne: false,
        whiteWine: false,
        redWine: false,
        whiskey: false,
        vodka: false,
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

    const normalizeText = (value) => value.trim().replace(/\s+/g, ' ').toLowerCase()

    const getStoredSubmissions = () => {
      try {
        const rawValue = localStorage.getItem(RSVP_STORAGE_KEY)
        const parsedValue = rawValue ? JSON.parse(rawValue) : []
        return Array.isArray(parsedValue) ? parsedValue : []
      } catch (error) {
        console.error('Failed to read RSVP submissions from localStorage', error)
        return []
      }
    }

    const saveStoredSubmissions = (submissions) => {
      try {
        localStorage.setItem(RSVP_STORAGE_KEY, JSON.stringify(submissions.slice(0, MAX_STORED_SUBMISSIONS)))
      } catch (error) {
        console.error('Failed to save RSVP submissions to localStorage', error)
      }
    }

    const formatDrinks = () => {
      const selected = []

      if (formData.drinks.champagne) selected.push('Шампанское')
      if (formData.drinks.whiteWine) selected.push('Белое вино')
      if (formData.drinks.redWine) selected.push('Красное вино')
      if (formData.drinks.whiskey) selected.push('Виски')
      if (formData.drinks.vodka) selected.push('Водка')

      return selected.length > 0 ? selected.join(', ') : 'Не выбрано'
    }

    const buildSubmissionSignature = () =>
      JSON.stringify({
        name: normalizeText(formData.name),
        attendance: formData.attendance || '',
        familyMembers: normalizeText(formData.familyMembers || ''),
        drinks: normalizeText(formatDrinks()),
        allergy:
          formData.allergy.hasAllergy === 'yes'
            ? normalizeText(formData.allergy.allergyDetails || '')
            : 'no',
        favoriteTrack: normalizeText(formData.favoriteTrack || ''),
        comment: normalizeText(formData.comment || ''),
      })

    const findDuplicateSubmission = () => {
      const signature = buildSubmissionSignature()
      return getStoredSubmissions().find((item) => item.signature === signature) || null
    }

    const rememberSubmission = () => {
      const signature = buildSubmissionSignature()
      const submissions = getStoredSubmissions().filter((item) => item.signature !== signature)

      submissions.unshift({
        signature,
        name: formData.name.trim(),
        submittedAt: new Date().toISOString(),
      })

      saveStoredSubmissions(submissions)
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

      const duplicateSubmission = findDuplicateSubmission()

      if (duplicateSubmission) {
        const submittedAt = new Date(duplicateSubmission.submittedAt).toLocaleString('ru-RU')

        $q.notify({
          type: 'warning',
          message: `Похоже, такая анкета уже была отправлена с этого устройства${submittedAt ? ` (${submittedAt})` : ''}.`,
          position: 'top-right',
          timeout: 5000,
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

        rememberSubmission()

        modalOpen.value = false

        formData.name = ''
        formData.attendance = null
        formData.familyMembers = ''
        formData.drinks = {
          champagne: false,
          whiteWine: false,
          redWine: false,
          whiskey: false,
          vodka: false,
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

    const checkMobile = () => {
      isMobile.value =
        /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
          navigator.userAgent,
        ) || window.innerWidth <= 768
    }

    onMounted(() => {
      checkMobile()
      window.addEventListener('resize', checkMobile)

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
      window.removeEventListener('resize', checkMobile)
    })

    return {
      container,
      isVisible,
      modalOpen,
      submitting,
      showTooltipDialog,
      isMobile,
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
