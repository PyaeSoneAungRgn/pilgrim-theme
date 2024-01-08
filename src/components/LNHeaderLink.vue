<script setup lang="ts">
import type { NavLink } from '../config'

defineProps<{
  item: NavLink
}>()
</script>

<template>
  <a
    :href="item.link"
    class="text-lg font-medium text-gray-600 hover:text-primary-500"
  >
    {{ item.text }}
  </a>
</template>
