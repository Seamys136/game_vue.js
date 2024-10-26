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
      }, 750); 
    },
    handleCatch() {
      this.opacity = 0; 
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
}
</style>