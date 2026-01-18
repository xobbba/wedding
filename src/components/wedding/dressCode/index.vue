<template>
  <div
    ref="container"
    class="dress row items-center justify-center bg-white"
    :class="{ 'animate': isVisible }"
  >
    <div class="text-dark text-center q-pa-xl">
      <!-- Заголовок -->
      <div class="text-h3 font-cormorant-sc q-mb-lg title-animate">
        DRESS CODE
      </div>

      <!-- Текст -->
      <div class="dress-text font-cormorant-sc q-mb-md text-block" data-index="1"
           :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
        Мы хотим создать особенную атмосферу на нашем торжестве, поэтому просим вас учесть дресс-код при выборе наряда
      </div>

      <div class="dress-text font-cormorant-sc q-mb-lg text-block" data-index="2"
           :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
        Будем признательны, если вы выберете наряды в коктейльном стиле.
      </div>

      <div class="dress-text font-cormorant-sc q-mb-xl text-block" data-index="3"
           :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
        Просим избегать белого, молочного цвета при выборе наряда.
      </div>

      <!-- Цветовая палитра -->
      <div class="color-palette">
        <div class="colors-grid">
          <div
            v-for="(color, index) in colors"
            :key="color.name"
            class="color-item"
            :data-index="index"
          >
            <div
              class="color-circle q-mb-md"
              :style="{ backgroundColor: color.hex }"
              :class="{ 'light-color': color.needsBorder }"
            ></div>
            <div class="font-cormorant-sc color-name" :class="!$q.screen.lt.md ? 'text-h6' : 'text-h5'">{{ color.name }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'DressCodeComponent',
  setup() {
    const $q = useQuasar()
    const container = ref(null)
    const isVisible = ref(false)
    let observer = null

    const colors = [
      { name: 'Лесной орех', hex: '#644D42', needsBorder: false },
      { name: 'Темный шоколад', hex: '#26140C', needsBorder: false },
      { name: 'Песчаный мед', hex: '#FFF3DF', needsBorder: true },
      { name: 'Пудровый беж', hex: '#D5C1B8', needsBorder: false }
    ]

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
        threshold: 0.2,
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
      isVisible,
      colors
    }
  }
})
</script>

<style lang="scss" scoped>
.dress {
  height: 800px;
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s ease-out;
}

/* Контейнер для текстовых блоков */
.dress-text {
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  opacity: 0;
  transform: translateX(-30px);
  transition: all 0.6s ease;

  &[data-index="2"] {
    transform: translateX(30px);
  }

  &[data-index="3"] {
    transform: translateX(-30px);
  }
}

/* Десктоп - грид с центрированием */
.colors-grid {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 48px;
}

.color-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  opacity: 0;
  transform: scale(0.8) rotate(-10deg);
  transition: all 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.color-circle {
  width: 160px;
  height: 160px;
  border-radius: 50%;
  transition: all 0.4s ease;
  position: relative;
  overflow: hidden;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: inherit;
    border-radius: 50%;
    transition: transform 0.5s ease;
  }

  &:hover {
    transform: scale(1.1) rotate(5deg);
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);

    &::before {
      transform: scale(1.1);
    }
  }
}

.color-circle.light-color {
  border: 3px solid #e0e0e0;

  &:hover {
    border-color: #c0c0c0;
  }
}

/* Название цвета - центрирование */
.color-name {
  text-align: center;
  max-width: 160px;
  word-wrap: break-word;
  opacity: 0;
  transform: translateY(10px);
  transition: all 0.4s ease 0.2s;
}

.title-animate {
  opacity: 0;
  transform: translateY(-20px);
  transition: all 0.8s ease;
}

.dress.animate {
  opacity: 1;
  transform: translateY(0);

  .title-animate {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.2s;
  }

  .dress-text[data-index="1"] {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.4s;
  }

  .dress-text[data-index="2"] {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.6s;
  }

  .dress-text[data-index="3"] {
    opacity: 1;
    transform: translateX(0);
    transition-delay: 0.8s;
  }

  .color-item[data-index="0"] {
    opacity: 1;
    transform: scale(1) rotate(0);
    transition-delay: 1s;
  }

  .color-item[data-index="1"] {
    opacity: 1;
    transform: scale(1) rotate(0);
    transition-delay: 1.2s;
  }

  .color-item[data-index="2"] {
    opacity: 1;
    transform: scale(1) rotate(0);
    transition-delay: 1.4s;
  }

  .color-item[data-index="3"] {
    opacity: 1;
    transform: scale(1) rotate(0);
    transition-delay: 1.6s;
  }

  .color-item[data-index="0"] .color-name {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1.1s;
  }

  .color-item[data-index="1"] .color-name {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1.3s;
  }

  .color-item[data-index="2"] .color-name {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1.5s;
  }

  .color-item[data-index="3"] .color-name {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 1.7s;
  }
}

/* Мобильные - круг слева, текст справа */
@media (max-width: 768px) {
  .dress {
    height: auto;
    min-height: 800px;
  }

  .dress-text {
    max-width: 90%;
  }

  .colors-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 40px;
    justify-items: center;
    align-items: center;
  }

  .color-item {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 25px;
    align-items: center;
    width: 100%;
    max-width: 350px;
    opacity: 0;
    transform: translateX(-30px);
    transition: all 0.6s ease;
  }

  .color-circle {
    width: 120px;
    height: 120px;
    margin: 0;
    order: 1;
  }

  /* Текст справа от круга */
  .color-name {
    text-align: left;
    max-width: none;
    margin: 0;
    padding: 0;
    word-break: break-word;
    line-height: 1.3;
    order: 2;
    transform: translateX(0);
    transition: all 0.4s ease 0.2s;
  }

  /* Ускоряем анимации на мобильных */
  .dress.animate {
    .title-animate { transition-delay: 0.1s; }
    .dress-text[data-index="1"] { transition-delay: 0.2s; }
    .dress-text[data-index="2"] { transition-delay: 0.3s; }
    .dress-text[data-index="3"] { transition-delay: 0.4s; }
    .color-item[data-index="0"] {
      opacity: 1;
      transform: translateX(0);
      transition-delay: 0.5s;
    }
    .color-item[data-index="1"] {
      opacity: 1;
      transform: translateX(0);
      transition-delay: 0.6s;
    }
    .color-item[data-index="2"] {
      opacity: 1;
      transform: translateX(0);
      transition-delay: 0.7s;
    }
    .color-item[data-index="3"] {
      opacity: 1;
      transform: translateX(0);
      transition-delay: 0.8s;
    }
    .color-item[data-index="0"] .color-name { transition-delay: 0.55s; }
    .color-item[data-index="1"] .color-name { transition-delay: 0.65s; }
    .color-item[data-index="2"] .color-name { transition-delay: 0.75s; }
    .color-item[data-index="3"] .color-name { transition-delay: 0.85s; }
  }
}

/* Маленькие экраны */
@media (max-width: 480px) {
  .dress {
    min-height: 700px;
  }

  .dress-text {
    max-width: 95%;
  }

  .color-item {
    max-width: 300px;
    gap: 20px;
  }

  .color-circle {
    width: 100px;
    height: 100px;
  }

  .color-name {
    font-size: 1.1rem;
  }
}

/* Очень маленькие экраны */
@media (max-width: 360px) {
  .colors-grid {
    gap: 30px;
  }

  .color-item {
    max-width: 280px;
    gap: 15px;
  }

  .color-circle {
    width: 85px;
    height: 85px;
  }

  .color-name {
    font-size: 1rem;
  }
}
</style>
