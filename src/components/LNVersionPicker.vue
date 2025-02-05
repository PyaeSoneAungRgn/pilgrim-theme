<script setup lang="ts">
import { Menu, MenuButton, MenuItem, MenuItems } from '@headlessui/vue'
import { ChevronDownIcon, CheckIcon } from '@heroicons/vue/20/solid'
import { useData } from 'vitepress'
import { computed } from 'vue'

const { page, theme } = useData()
const versions = computed(() => theme.value.versions)
const currentVersion = computed(() => versions.value.filter(v => v.current)[0])
</script>

<template>
  <Menu v-if="versions.length > 1" as="div" class="relative inline-block text-left">
    <div>
      <MenuButton
        class="inline-flex w-full items-center justify-center gap-x-1.5 rounded-full bg-gray-100 px-3 py-1 text-[13px] font-semibold text-gray-500 hover:bg-gray-200">
        {{ currentVersion.text }}
        <ChevronDownIcon class="text-gray-500 w-4 h-4 ml-2 -mr-1" aria-hidden="true" />
      </MenuButton>

      <MenuItems
        class="absolute right-0 z-10 mt-2 w-28 origin-top-right rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
        <div class="py-1">
          <MenuItem v-for="{ link, text, current } in versions" v-slot="{ active }">
          <a :href="link"
            class="flex items-center gap-2 w-full font-semibold text-gray-600 text-left px-3 py-1 text-sm hover:bg-gray-50"
            :class="{ '!text-primary-500': current }">
            {{ text }}
            <CheckIcon v-if="current" class="ml-auto w-4" />
          </a>
          </MenuItem>
        </div>
      </MenuItems>
    </div>
  </Menu>
  <div v-else>
    <span
      class="inline-flex w-full items-center justify-center gap-x-1.5 rounded-full bg-gray-100 px-3 py-1 text-[13px] font-semibold text-gray-500 hover:bg-gray-200">
      {{ version }}
    </span>
  </div>
</template>
