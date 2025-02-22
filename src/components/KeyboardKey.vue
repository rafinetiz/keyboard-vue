<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{
  keymap: Record<string, {
    state: 0 | 1;
    pressed: boolean;
  }>;
  code: string;
  label: string;
  grow?: string;
}>();

const style_attr = computed(() => {
  if (props.grow) {
    return {
      'flex-grow': props.grow,
      'width': 'unset'
    }
  }

  return {}
});
</script>

<template>
  <div class="keycaps"
    :class="{ 'active': keymap[code] && keymap[code].state === 1, 'pressed': keymap[code] && keymap[code].pressed }"
    :style="style_attr">

    <span>{{ label }}</span>
  </div>
</template>