<script setup lang="ts">
import LNLogo from './LNLogo.vue'
import LNVersionPicker from './LNVersionPicker.vue'
import LNSidebar from './LNSidebar.vue'
import {
  Bars3Icon,
  EllipsisVerticalIcon,
  MagnifyingGlassIcon,
} from '@heroicons/vue/24/outline'
import {
  breakpointsTailwind,
  useBreakpoints,
  useEventListener,
} from '@vueuse/core'
import { Dialog, DialogPanel } from '@headlessui/vue'
import { onContentUpdated, useData } from 'vitepress'
import { computed, ref } from 'vue'
import LNGitHubLink from './LNGitHubLink.vue'
import LNHeaderLink from './LNHeaderLink.vue'

const { site, theme } = useData()

const isOpen = ref(false)
const dotMenuOpen = ref(false)

const githubUrl = computed(() => theme.value.githubUrl)

onContentUpdated(() => {
  setIsOpen(false)
  setDotMenuOpen(false)
})
const breakpoints = useBreakpoints(breakpointsTailwind)
const largerThanSm = breakpoints.greater('md')

useEventListener('resize', () => {
  if (largerThanSm.value) {
    setIsOpen(false)
    setDotMenuOpen(false)
  }
})

function openSearch() {
  window.dispatchEvent(
    new KeyboardEvent('keydown', { key: 'k', metaKey: true })
  )
}

const navItems = computed(() => theme.value.nav)
const shouldShowGitHubLink = computed(() => theme.value.githubUrl != false)
const shouldShowNavItems = computed(() => theme.value.nav?.length > 0)
const shouldShowVersionPicker = computed(() => theme.value.versions?.length > 1)

function setIsOpen(value: boolean): void {
  isOpen.value = value
}

function setDotMenuOpen(value: boolean): void {
  dotMenuOpen.value = value
}
</script>

<template>
  <header
    class="bg-white/80 backdrop-blur border-b sticky top-0 z-50 w-full h-[3.3rem] flex-none"
  >
    <div class="max-w-[90rem] px-4 sm:px-6 md:px-8 mx-auto h-[3.3rem]">
      <div class="flex items-center h-[3.3rem] gap-4">
        <button
          type="button"
          @click="setIsOpen(true)"
          class="md:hidden text-gray-600"
        >
          <span class="sr-only">Navigation</span>
          <Bars3Icon class="flex-shrink-0 w-6" />
        </button>

        <Dialog :open="isOpen" @close="setIsOpen" class="relative z-50">
          <div
            class="fixed inset-0 bg-white/25 backdrop-blur"
            aria-hidden="true"
          />

          <div class="fixed inset-0 overflow-y-auto">
            <div class="flex min-h-full">
              <DialogPanel
                class="bg-white shadow p-6 w-[230px]"
              >
                <LNSidebar />
              </DialogPanel>
            </div>
          </div>
        </Dialog>

        <slot name="logo">
          <LNLogo
            class="flex-shrink-0 h-[24px] text-gray-500"
            :alt="site.title"
          />
        </slot>

        <LNVersionPicker v-if="shouldShowVersionPicker" />

        <div
          class="ml-auto md:flex items-center md:divide-x md:divide-gray-200"
        >
          <div
            v-if="shouldShowNavItems"
            class="hidden md:flex items-center gap-12 px-6"
          >
            <a
              v-for="{ text, link } in navItems"
              :href="link"
              class="text-sm font-medium text-gray-600 hover:text-primary-500"
            >
              {{ text }}
            </a>
          </div>

          <div class="hidden md:flex md:items-center gap-6 pl-6">
            <LNGitHubLink v-if="shouldShowGitHubLink" />
          </div>

          <div class="flex items-center gap-3 pl-6">
            <button
              type="button"
              @click="openSearch"
              class="md:hidden flex-shrink-0 text-gray-600"
            >
              <span class="sr-only">Search</span>
              <MagnifyingGlassIcon class="w-6" />
            </button>

            <button
              @click="setDotMenuOpen(true)"
              type="button"
              class="md:hidden flex-shrink-0 text-gray-600"
            >
              <span class="sr-only">More</span>
              <EllipsisVerticalIcon class="w-6" />
            </button>

            <Dialog
              :open="dotMenuOpen"
              @close="setDotMenuOpen"
              class="relative z-50"
            >
              <div
                class="fixed inset-0 bg-white/25 backdrop-blur"
                aria-hidden="true"
              />

              <div class="fixed inset-0 overflow-y-auto">
                <div class="flex justify-end p-6">
                  <DialogPanel
                    class="bg-white shadow p-8 w-[330px] space-y-8 rounded-lg divide-y divide-gray-200"
                  >
                    <div
                      v-if="shouldShowNavItems || shouldShowGitHubLink"
                      class="flex flex-col md:hidden gap-4"
                    >
                      <LNHeaderLink
                        v-if="shouldShowNavItems"
                        v-for="item in navItems"
                        :item="item"
                      />
                      <LNHeaderLink
                        v-if="shouldShowGitHubLink"
                        :item="{ text: 'GitHub', link: githubUrl }"
                      />
                    </div>
                  </DialogPanel>
                </div>
              </div>
            </Dialog>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>
