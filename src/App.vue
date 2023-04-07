<script setup lang="ts">
import { onMounted } from "vue";
import { RouterView, useRouter } from 'vue-router'

const router = useRouter();
onMounted(() => {
  // Github pages SPA workaround
  let path = localStorage.getItem("path");
  if (path) {
    localStorage.removeItem("path");
    router.push(path);
  }
});
</script>

<template>
  <RouterView />
</template>

<style>
@import '@/assets/base.css';

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
  background-color: #f5f5f5;
  overflow: hidden;
}

.no-highlights {
  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style>
