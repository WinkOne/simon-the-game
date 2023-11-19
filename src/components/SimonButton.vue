<template>
  <div class="simon-button" @click="handleClick" :class="{ active: isActive }"></div>
</template>

<script>
export default {
  props: {
    playSound: Function,
  },
  data() {
    return {
      isActive: false,
    };
  },
  methods: {
    handleClick() {
      this.isActive = true;
      this.playSound();
      setTimeout(() => {
        this.isActive = false;
      }, 500);
    },
  },
};
</script>

<style scoped>
.simon-button {
  width: 100px;
  height: 100px;
  background-color: #3498db;
  margin: 10px;
  cursor: pointer;
}

.active {
  background-color: #2980b9;
}
</style>
