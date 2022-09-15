<script setup lang="ts">
import { isDark, toggleDark } from '~/composables'
const router = useRouter()
const backhome = () => {
  router.push('/')
}
</script>

<template>
  <nav text-xl mt-6 inline-flex gap-2>
    <button class="icon-btn !outline-none" @click="toggleDark()">
      <div v-if="isDark" i-carbon-moon />
      <div v-else i-carbon-sun />
    </button>

    <a
      class="icon-btn"
      i-carbon-home
      rel="noreferrer"
      title="GitHub"
      @click="backhome"
    />
  </nav>
</template>
