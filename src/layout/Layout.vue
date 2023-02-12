<script lang="ts">
import Home from '@/pages/home/Home.vue'


export default {
  components: { Home },
}
</script>

<template>
  <van-config-provider theme="light">
    <div class="layout">
      <Home />
    </div>
  </van-config-provider>
</template>

<style scoped lang="scss">
.layout {
  min-height: 100vh;
  background-color: rgb(247, 248, 250);
}
</style>
