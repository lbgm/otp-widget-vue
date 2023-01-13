<script setup lang="ts">
import { ref } from "vue";
import type { Ref } from "vue";
import OtpWidget from './components/otp-widget.vue'

const otp: Ref<string> = ref("");
</script>

<template>
  <main>
    <otp-widget  @code="otp = $event" :childs="6" />
  </main>
</template>

<style scoped lang="scss">
//
</style>
