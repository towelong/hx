<script setup lang="ts">
import axios from 'axios'

const props = defineProps<{ id: string }>()
interface ICoupon {
  id: number
  coupon_uuid: string
  qrcode: string
  used: number
}
const item = ref<ICoupon>()

onMounted(async () => {
  const res = await axios.get(`https://api.fdrfu.cn/coupon/${props.id}`)
  item.value = res.data
})
</script>

<template>
  <div>
    <img :src="item?.qrcode" alt="coupon">
  </div>
</template>

<style scoped>
</style>
