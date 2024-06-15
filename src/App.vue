<script setup lang="ts">
import { RouterView } from 'vue-router'

import { computed } from 'vue';
const route = useRoute();
const layout = computed(() => {
  return route.meta.layout ? route.meta.layout + '-layout' : 'default-layout';
});
</script>

<template>
  <component :is="layout">
    <RouterView />
  </component>
</template>
