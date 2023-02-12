<script lang="ts">
import Layout from './layout/Layout.vue'
import { ref, defineComponent } from 'vue'


export default defineComponent({
  components: { Layout },

  setup () {
    const width = ref(document.documentElement.clientWidth || document.body.clientWidth)
    const isPhone = ref(false)
    const noShowIframe = ref(isPhone.value || width.value <= 750)

    const info = navigator.userAgent
    isPhone.value = /mobile/i.test(info)

    const src = ref(`${window.location.protocol}//${window.location.host}/index.html`)

    return { noShowIframe, src }
  }
})
</script>

<template>
  <div id="app_box">
    <div v-if="noShowIframe" id="box" :style="{width: noShowIframe? '100vw' : '800px'}">
      <Layout />
    </div>
    <div v-else style="width: 750px;">
      <iframe style="width: 750px; height: 100vh;" :src="src" />
    </div>
  </div>
</template>

<style scoped lang="scss">
#app_box{
  display: flex;
  justify-content: center;
  align-items: center;
  // width: 100vw;
  height: 100vh;
  overflow: hidden;
  #box{
    overflow: hidden;
    height: 100vh;
  }
}
</style>
