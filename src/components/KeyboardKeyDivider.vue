<script setup lang="ts">
defineProps<{
  grow: string;
}>();

</script>
<template>
  <div :style="{ 'flex-grow': grow }"></div>
</template>