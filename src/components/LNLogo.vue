<script setup lang="ts">
import { useData, withBase } from 'vitepress'
const { theme } = useData()
</script>

<template>
  <a :href="withBase('/')">
    <img v-bind="$attrs" :src="withBase(theme.logo.light)" alt="logo" />
  </a>
</template>
