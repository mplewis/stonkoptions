<script setup lang="ts">
import { computed, ref } from 'vue'

defineProps<{ msg: string }>()

const count = ref(0)

const min = 1_000_000 // 1m
const max = 100_000_000_000 // 100b
const steps = 50

const rawSlider = ref(0)
function onChange(event: any) {
  rawSlider.value = parseInt(event.target.value) / 1000
}
const slider = computed(() => {
  return Math.pow(10, rawSlider.value)
})
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <input type="range" @input="onChange" :min="Math.log10(min) * 1000" :max="Math.log10(max) * 1000"
      :step="(Math.log10(max) - Math.log10(min)) / steps">
    <p>
      slider:
      {{ new Intl.NumberFormat('en-US', {
          maximumSignificantDigits: 3, style: 'currency', currency: 'USD'
        }).format(slider)
      }}
    </p>
  </div>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank">create-vue</a>, the official Vue + Vite
    starter
  </p>
  <p>
    Install
    <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
