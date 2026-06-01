<script setup>
import { ref } from 'vue'
import BaseButton from './ui/BaseButton.vue'
import NavButton from './ui/NavButton.vue'
import ThemeToggle from './ui/ThemeToggle.vue'

defineProps({
  sections: {
    type: Array,
    default: () => [],
  },
  activeSection: {
    type: String,
    default: '',
  },
  isDarkMode: {
    type: Boolean,
    default: false,
  },
  frameGradientStyle: {
    type: Object,
    default: () => ({}),
  },
})

const emit = defineEmits(['select-section', 'toggle-theme'])
const isMobileMenuOpen = ref(false)

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

const selectSection = (sectionId) => {
  emit('select-section', sectionId)
  closeMobileMenu()
}
</script>

<template>
  <header class="fixed left-0 right-0 top-0 z-40 transition-colors duration-500"
    :class="isDarkMode ? 'bg-zinc-950 text-white' : 'bg-white text-black'">
    <div class="absolute inset-x-0 bottom-0 h-1 transition-[background-position] duration-700 ease-out"
      :style="frameGradientStyle"></div>
    <nav class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
      <div class="flex h-16 items-center justify-between sm:h-20">
        <button type="button"
          class="relative cursor-pointer font-['Press_Start_2P'] text-sm tracking-tight transition duration-200 hover:-translate-y-0.5 hover:text-cyan-400 hover:[text-shadow:0_0_6px_#22d3ee,0_0_16px_#22d3ee,2px_2px_0_#ec4899] focus-visible:outline-4 focus-visible:outline-offset-4 focus-visible:outline-yellow-300 sm:text-base"
          @click="selectSection('home')">
          <span class="text-cyan-400">Wojiin</span>.DEV
        </button>

        <div class="hidden items-center gap-8 md:flex">
          <NavButton v-for="section in sections" :key="section.id" :active="activeSection === section.id"
            @click="selectSection(section.id)">
            {{ section.label }}
          </NavButton>
        </div>

        <div class="flex items-center gap-4">
          <ThemeToggle :dark-mode="isDarkMode" @toggle="$emit('toggle-theme')">
            <template #thumb>{{ isDarkMode ? '☾' : '☀' }}</template>
          </ThemeToggle>

          <BaseButton compact :dark-mode="isDarkMode" class="md:hidden" @click="toggleMobileMenu">
            ☰
          </BaseButton>
        </div>
      </div>
    </nav>

    <div v-if="isMobileMenuOpen" class="relative px-4 py-4 md:hidden" :class="isDarkMode ? 'bg-zinc-950' : 'bg-white'">
      <div class="absolute inset-x-0 top-0 h-1 transition-[background-position] duration-700 ease-out"
        :style="frameGradientStyle"></div>
      <div class="space-y-4">
        <NavButton v-for="section in sections" :key="section.id" mobile :active="activeSection === section.id"
          @click="selectSection(section.id)">
          {{ section.label }}
        </NavButton>
      </div>
    </div>
  </header>
</template>
