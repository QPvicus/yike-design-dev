<template>
  <yk-space direction="vertical">
    <yk-input v-model:value="words" size="s" style="width: 360px" clearable />
    <yk-input v-model:value="words" size="m" style="width: 360px" />
    <yk-input v-model:value="words" size="l" style="width: 360px" />
    <yk-input v-model:value="words" size="xl" style="width: 360px" />
  </yk-space>
</template>
<script setup lang="ts">
import { ref } from 'vue'
const words = ref<string>('你好，Yike Design')
</script>
