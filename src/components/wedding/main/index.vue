<template>
  <div class="fullscreen row items-start justify-start bg-image">
    <div class="text-white" :class="!$q.screen.lt.md ? 'q-pt-xl' : 'custom-padding'">
      <div
        class="font-cormorant-sc line-1"
        :class="!$q.screen.lt.md ? 'text-h2' : 'text-h4'"
      >
        KONSTANTIN
      </div>

      <div
        class="font-cormorant-sc line-2"
        :class="!$q.screen.lt.md ? 'text-h2' : 'text-h4'"
      >
        & VERONIKA
      </div>

      <div
        class="font-cormorant-infant line-3"
        :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'"
      >
        28|08|2026
      </div>
    </div>

    <div class="bottom-image-container">
      <img
        src="/img/up.png"
        alt="Decoration"
        class="bottom-image"
      />
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'MainComponent'
})
</script>

<style lang="scss" scoped>
.bg-image {
  background-image: url('/img/we.png');
  background-repeat: no-repeat;
  background-position: 50% 10%;
  background-size: cover;
  position: relative;
  height: 1200px;
}

.bottom-image-container {
  position: absolute;
  bottom: -20px;
  left: 0;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  z-index: 3;
}

.bottom-image {
  width: 100%;
  max-width: 100%;
  height: auto;
  display: block;
}
@mixin text-appear($delay: 0s) {
  opacity: 0;
  transform: translateY(30px);
  animation: textAppear 1s ease-out forwards $delay;
}

@keyframes textAppear {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

// Стили с анимациями
.line-1 {
  margin-left: 120px;
  margin-bottom: 5px;
  letter-spacing: 4px;
  margin-top: 100px;
  @include text-appear(0.3s);
}

.line-2 {
  margin-left: 256px;
  letter-spacing: 4px;
  @include text-appear(0.6s);
}

.line-3 {
  margin-left: 532px;
  margin-top: 16px;
  letter-spacing: 2px;
  @include text-appear(0.9s);
}

// Адаптивность
@media (max-width: 1440px) {
  .bg-image {
    height: 900px;
  }

  .line-1 {
    margin-left: 100px;
    margin-top: 80px;
  }
  .line-2 { margin-left: 150px; }
  .line-3 { margin-left: 125px; }
}

@media (max-width: 1024px) {
  .bg-image {
    height: 768px;
  }

  .line-1 {
    margin-left: 80px;
    margin-top: 60px;
  }
  .line-2 { margin-left: 120px; }
  .line-3 { margin-left: 100px; }

  .line-3 {
    letter-spacing: 6px;
  }
}

@media (max-width: 768px) {
  .bg-image {
    height: 600px;
  }

  .line-1 {
    margin-left: 20px;
    letter-spacing: 2px;
    margin-top: 40px;
    animation-delay: 0.1s !important;
  }

  .line-2 {
    margin-left: 76px;
    letter-spacing: 2px;
    animation-delay: 0.3s !important;
  }

  .line-3 {
    margin-left: 120px;
    letter-spacing: 4px;
    margin-top: 2px;
    animation-delay: 0.5s !important;
  }
}

.custom-padding {
  padding-top: 168px;
}
</style>
