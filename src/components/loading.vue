<template>
  <div class="loader-screen">
    <img class="loader-logo" :src="logo" alt="Logo" />
    <h1 class="loader-title">CPTM X FATEC</h1>
    <div class="spinner"></div>
  </div>
</template>
<script>
import { ref, onMounted } from "vue";
import logo from "../assets/logo.png";

export default {
  setup() {
    const img = new Image()
    img.src = logo

    const loading = ref(true);

    onMounted(() => {
      const img = new Image();
      img.src = logo;

      setTimeout(() => {
        loading.value = false;
      }, 3000);
    });

    return { loading, logo };
  },
};
</script>
<style>
.loader-screen {
  position: fixed;
  inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #fff;
  z-index: 9999;
}

.loader-logo {
  width: 250px;
  height: auto;
  margin-bottom: 10px;
}

.loader-title {
  font-size: 2rem;
  margin-bottom: 1rem;
  color: #333;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #ccc;
  border-top-color: #fff;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.app-content {
  padding: 2rem;
}
</style>
