<script setup lang="ts">
  import { onMounted, ref } from 'vue'
  import { useDetectUpdate } from 'unplugin-detect-update/hooks'

  const version = ref()

  const changed = ref()

  const { onUpdate } = useDetectUpdate({
    ms: 5000
  })

  onUpdate(async () => {
    const confirmed = await confirm('Need Update!')
    confirmed && location.reload()
  })

  onMounted(() => {
    version.value = document.body.getAttribute('version') ?? '0.0.0'
    changed.value = version.value
  })

  const random = () => Math.floor(Math.random() * 10)

  const change = () => {
    changed.value = `${random()}.${random()}.${random()}`
    const version = { version: changed.value }
    sessionStorage.setItem('detect-update-store', JSON.stringify(version))
  }
</script>

<template>
  <div>
    <h2>vite example</h2>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
    <h3>Current Version: {{ version }}</h3>

    <button @click="change">Change To {{ changed }}</button>
  </div>
</template>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.vue:hover {
    filter: drop-shadow(0 0 2em #42b883aa);
  }
</style>
