<script setup lang="ts">
import { ref } from "vue";
import type { Ref } from "vue";
import OtpWidget from './components/otp-widget.vue'

const otp: Ref<string> = ref("");
</script>

<template>
  <main>
    <div>
      <otp-widget  @code="otp = $event" :childs="6" />
    </div>
  </main>
</template>

<style scoped lang="scss">
//
main {
  height: 100vh;
  widows: 100vw;

  :deep(.widget-otp-parent) {
    //
  }
}
</style>
