<template>
  <div
    class="fly"
    :style="flyStyle"
    @click="handleCatch"
    v-if="isVisible">
    <img :src="flyImg" alt="Муха" />
  </div>
</template>

<script>
export default {
  props: {
    flyImg: {
      type: String,
      required: true 
    }
  },
  data() {
    return {
      flyPosition: { 
        x: Math.random() * window.innerWidth, 
        y: Math.random() * window.innerHeight 
      },
      isVisible: true, 
      opacity: 1, 
      angle: 0, 
      movementInterval: null 
    };
  },
  mounted() {
    this.startMoving(); 
  },
  beforeDestroy() {
    clearInterval(this.movementInterval); 
  },
  computed: {
    flyStyle() {
      return {
        top: this.flyPosition.y + 'px',
        left: this.flyPosition.x + 'px',
        opacity: this.opacity,
        transform: `rotate(${this.angle}deg)` 
      };
    }
  },
  methods: {
    startMoving() {
      this.movementInterval = setInterval(() => {
        this.flyPosition.x = Math.random() * (window.innerWidth - 50); 
        this.flyPosition.y = Math.random() * (window.innerHeight - 50);
        this.angle += Math.random() * 20 - 10; 
      }, 1000); 
    },
    handleCatch() {
      this.opacity = 0; 
      const flyElement = this.$el; 
      flyElement.classList.add('fade-out'); 

      setTimeout(() => {
        this.isVisible = false; 
        this.$emit('catch'); 
      }, 500);
    }
  }
};
</script>

<style scoped>
.fly {
  position: absolute;
  transition: opacity 0.5s, transform 0.1s;
  animation: flyIn 0.5s ease-in-out;
}

@keyframes flyIn {
  from {
    transform: scale(0);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.fly.fade-out {
  animation: fadeOut 0.5s forwards;
}

@keyframes fadeOut {
  0% {
    opacity: 1;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.5;
  }
  100% {
    opacity: 0;
    transform: scale(0);
  }
}
</style>