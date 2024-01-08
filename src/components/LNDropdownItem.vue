<script setup lang="ts">
import { MenuItem } from '@headlessui/vue'
</script>

<template>
  <MenuItem v-slot="{ active }">
  <button type="button" :class="[
    'flex items-center gap-3 w-full font-semibold text-gray-700 text-left px-3 py-1 text-sm hover:bg-gray-50',
  ]">
    <slot />
  </button>
  </MenuItem>
</template>
