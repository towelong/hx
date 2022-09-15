<script setup lang="ts">
import { QrcodeStream } from 'qrcode-reader-vue3'
import axios from 'axios'

const camera = ref('auto')
// const result = ref('')
const showScanConfirmation = ref(false)

async function check(id: string) {
  const res = await axios.post('https://api.fdrfu.cn/coupon/check', {
    coupon_uuid: id,
  })
  if (res.data.msg === 'success')
    alert('核销成功')
  else
    alert('核销失败')
}

async function onInit(promise: any) {
  try {
    await promise
  }
  catch (e) {
    console.error(e)
  }
  finally {
    showScanConfirmation.value = camera.value === 'off'
  }
}

async function onDecode(content: string) {
  // pause()
  showScanConfirmation.value = true
  await check(content)
  // await timeout(1500)
  unpause()
  showScanConfirmation.value = false
}

function unpause() {
  camera.value = 'auto'
}

// function pause() {
//   camera.value = 'off'
// }

// function timeout(ms: number) {
//   return new Promise((resolve) => {
//     window.setTimeout(resolve, ms)
//   })
// }
</script>

<template>
  <div flex flex-col items-center>
    <!-- <p class="decode-result">
      Last result: <b>{{ result }}</b>
    </p> -->
    <div class="w-50 h-50">
      <QrcodeStream :camera="camera" @decode="onDecode" @init="onInit">
        <div v-if="showScanConfirmation" class="scan-confirmation">
          <img src="/checkmark.svg" alt="Checkmark" width="128" height="128">
        </div>
      </QrcodeStream>
    </div>
  </div>
</template>

<style scoped>
.scan-confirmation {
  position: absolute;
  width: 100%;
  height: 100%;

  background-color: rgba(255, 255, 255, .8);

  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
}
</style>
