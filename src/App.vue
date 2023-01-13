<script setup lang="ts">
import { ref } from "vue";
import type { Ref } from "vue";
import OtpWidget from './components/otp-widget.vue'

const otp: Ref<string> = ref("");
</script>

<template>
  <main>
    <div class="otp-container">
      <span class="log">{{ otp }}</span>
      <otp-widget @filled="void 0" @code="otp = $event" :childs="4" placeholder="" :gap="'otp-gap-16'" />
    </div>
  </main>
</template>

<style scoped lang="scss">
//
main {
  height: 100vh;
  widows: 100vw;

  .otp-container {
    padding: 98px 0;

    .log {
      display: block;
      margin: 24px 0;
      text-align: center;
      font-size: 12px;
      line-height: 14px;
      font-family: 'Arial';
    }
  }

  :deep(.widget-otp-parent) {
    //
    opacity: 0.9;
  }
}
</style>
