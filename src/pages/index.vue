<script setup lang="ts">
const router = useRouter()
const go = (url: string) => {
  router.push(url)
}
</script>

<template>
  <div class="flex flex-col gap-2">
    <div>
      <button
        class="btn"
        @click="go('/gen')"
      >
        生成券码
      </button>
    </div>
    <div>
      <button
        class="btn"
        @click="go('/scan')"
      >
        核销券码
      </button>
    </div>
  </div>
</template>
