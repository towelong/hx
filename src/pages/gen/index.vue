<script setup lang="ts">
import axios from 'axios'

interface ICoupon {
  id: number
  coupon_uuid: string
  qrcode: string
  used: number
}

const list = ref<ICoupon[]>([])
const count = ref<number>()
const router = useRouter()

const getCouponList = async () => {
  const res = await axios.get('https://api.fdrfu.cn/coupon')
  list.value = res.data
}

onMounted(async () => {
  await getCouponList()
})

const getQrcode = (id: string) => {
  router.push(`/coupon/${id}`)
}

const generate = async () => {
  if (count.value && count.value > 0) {
    const res = await axios.post('https://api.fdrfu.cn/coupon', {
      count: count.value,
    })
    if (res.data.msg === 'success') {
      await getCouponList()
      count.value = undefined
    }
  }
  else {
    alert('请填入正确的数量')
  }
}
</script>

<template>
  <div class="flex flex-col gap-2 items-center">
    <input
      v-model="count" type="text" autocomplete="false" p="x4 y2" w="250px" text="center" bg="transparent"
      border="~ rounded gray-200 dark:gray-700" outline="none active:none" placeholder="输入生成的券码数量"
    >
    <div>
      <button class="btn" @click="generate">
        生成券码
      </button>
    </div>
    <div>已生成的券码列表</div>
    <div class="flex flex-col gap-2">
      <div v-for="item in list" :key="item.id" @click="getQrcode(item.coupon_uuid)">
        <!-- <img :src="item.qrcode"> -->
        <p class="border-b-1">
          {{ item.coupon_uuid }} - {{ item.used === 1 ? '已使用' : '未使用' }}
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
