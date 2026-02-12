<template>
  <div class="fullscreen row items-start justify-start bg-image" :class="{ loaded: isLoaded }">
    <div class="text-white" :class="!$q.screen.lt.md ? 'q-pt-xl' : 'custom-padding'">
      <div class="font-cormorant-sc line-1" :class="!$q.screen.lt.md ? 'text-h2' : 'text-h3'">
        KONSTANTIN
      </div>

      <div class="font-cormorant-sc line-2" :class="!$q.screen.lt.md ? 'text-h2' : 'text-h3'">
        & VERONIKA
      </div>

      <div class="font-cormorant-infant line-3" :class="!$q.screen.lt.md ? 'text-h4' : 'text-h5'">
        28|08|2026
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'

export default defineComponent({
  name: 'MainComponent',
  setup() {
    const isLoaded = ref(false)

    onMounted(() => {
      setTimeout(() => {
        isLoaded.value = true
      }, 300)
    })

    return {
      isLoaded,
    }
  },
})
</script>

<style lang="scss" scoped>
.bg-image {
  background-image: url('/img/we.png');
  background-repeat: no-repeat;
  background-position: 50% 60%;
  background-size: cover;
  position: relative;
  height: 1200px;
  z-index: 2;

  opacity: 0;
  transform: scale(1.05);
  transition: all 1.5s cubic-bezier(0.4, 0, 0.2, 1);

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.3);
    opacity: 1;
    transition: opacity 1.2s ease 0.5s;
    z-index: 1;
  }

  &.loaded {
    opacity: 1;
    transform: scale(1);

    &::before {
      opacity: 0;
    }
  }
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

  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease 1.2s;

  .bg-image.loaded & {
    opacity: 1;
    transform: translateY(0);
  }
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

.line-1 {
  margin-left: 120px;
  margin-bottom: 5px;
  letter-spacing: 4px;
  margin-top: 100px;
  @include text-appear(0.3s);

  animation-delay: 0.8s !important;
}

.line-2 {
  margin-left: 256px;
  letter-spacing: 4px;
  @include text-appear(0.6s);
  animation-delay: 1.1s !important;
}

.line-3 {
  margin-left: 532px;
  margin-top: 16px;
  letter-spacing: 2px;
  @include text-appear(0.9s);
  animation-delay: 1.4s !important;
}

.bg-image.loaded {
  .line-1,
  .line-2,
  .line-3 {
    text-shadow: 0 0 10px rgba(255, 255, 255, 0.1);
    transition: text-shadow 0.5s ease 0.5s;

    &:hover {
      text-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
    }
  }
}

@media (max-width: 1440px) {
  .bg-image {
    height: 900px;
  }

  .line-1 {
    margin-left: 100px;
    margin-top: 80px;
  }
  .line-2 {
    margin-left: 150px;
  }
  .line-3 {
    margin-left: 125px;
  }
}

@media (max-width: 1024px) {
  .bg-image {
    height: 768px;
  }

  .line-1 {
    margin-left: 80px;
    margin-top: 60px;
  }
  .line-2 {
    margin-left: 120px;
  }
  .line-3 {
    margin-left: 100px;
  }

  .line-3 {
    letter-spacing: 6px;
  }
}

@media (max-width: 768px) {
  .bg-image {
    height: 600px;
    background-size: cover;
  }

  .line-1 {
    margin-left: 20px;
    letter-spacing: 2px;
    margin-top: 40px;
    animation-delay: 0.6s !important;
  }

  .line-2 {
    margin-left: 76px;
    letter-spacing: 2px;
    animation-delay: 0.9s !important;
  }

  .line-3 {
    margin-left: 90px;
    letter-spacing: 4px;
    margin-top: 2px;
    animation-delay: 1.2s !important;
  }

  .custom-padding {
    padding-top: 2px;
  }
}

@media (max-width: 480px) {
  .bg-image {
    height: 500px;
    background-size: 150% auto;
    background-position: 60% 55%;

    &.loaded {
      &::before {
        transition-delay: 0.3s;
      }
    }
  }

  .bottom-image-container {
    transition-delay: 0.8s;
  }
}

@media (prefers-reduced-motion: reduce) {
  .bg-image,
  .bottom-image-container,
  .line-1,
  .line-2,
  .line-3 {
    animation: none !important;
    transition: none !important;
    opacity: 1 !important;
    transform: none !important;
  }

  .bg-image::before {
    display: none;
  }
}
</style>
