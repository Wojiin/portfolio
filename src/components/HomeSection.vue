<script setup>
import BaseButton from './ui/BaseButton.vue'

defineProps({
  isDarkMode: {
    type: Boolean,
    default: false,
  },
  sections: {
    type: Array,
    default: () => [],
  },
  activeSection: {
    type: String,
    default: '',
  },
})

const emit = defineEmits(['select-section'])

const handleSectionClick = (sectionId) => {
  emit('select-section', sectionId)
}
</script>

<template>
  <section id="home" class="relative flex h-full overflow-hidden bg-transparent  py-70 "
    :class="isDarkMode ? 'border-cyan-400' : 'border-black'">
    <div class="absolute left-8 top-28 z-[1] hidden h-20 w-20 rotate-45 border-4 border-pink-500 sm:block"></div>
    <div class="absolute left-[calc(50%+14rem)] top-[15.75rem] z-[1] hidden h-28 w-28 lg:block" :class="isDarkMode
      ? 'rounded-full bg-[radial-gradient(circle,#fef08a_0%,#facc15_24%,#f59e0b_52%,#f97316_78%,#ea580c_100%)] shadow-[0_0_64px_rgba(249,115,22,0.78),0_0_140px_rgba(234,88,12,0.56)]'
      : 'bg-yellow-300'"></div>
    <div class="absolute bottom-0 right-20 z-[1] hidden h-16 w-16 border-4 border-cyan-400 lg:block"></div>

    <div
      class="relative z-10 mx-auto flex h-full w-full max-w-7xl flex-col items-center justify-center gap-10 px-4 py-10 sm:px-6 sm:py-12 lg:flex-row lg:gap-16 lg:px-8">
      <div class="relative z-0">
        <div
          class="relative z-10 flex h-64 w-64 items-center justify-center overflow-hidden border-8 border-black bg-gradient-to-br from-cyan-400 to-pink-500 sm:h-80 sm:w-80">
          <img src="/img/homeheroportrait.png" alt="Portrait de Wojiin" class="h-full w-full object-cover" />
          <div
            class="pointer-events-none absolute inset-0 bg-[repeating-linear-gradient(0deg,rgba(0,0,0,0.1)_0,rgba(0,0,0,0.1)_1px,transparent_1px,transparent_2px)]">
          </div>
          <div class="absolute inset-0 overflow-hidden">
            <div class="h-2 w-full animate-pulse bg-white/15"></div>
          </div>
        </div>
        <div class="absolute -bottom-4 -right-4 z-0 h-full w-full border-4 border-pink-500"></div>
      </div>

      <div class="text-center lg:text-left">
        <p class="mb-4 font-['Press_Start_2P'] text-xs tracking-widest text-cyan-400 sm:text-sm">HELLO, I'M</p>
        <h1 class="text-5xl font-bold tracking-tight sm:text-7xl lg:text-8xl">
          Wojiin

          <div class="mt-4 h-2 w-96 bg-yellow-300"></div>

        </h1>
        <p class="mb-8 mt-6 font-['Press_Start_2P'] text-sm text-pink-500 [text-shadow:2px_2px_0_#22d3ee] sm:text-base">
          Prêtre chill de Miami
        </p>
        <p class="mx-auto mb-8 max-w-xl font-[Inter] text-base leading-7 sm:text-lg lg:mx-0">
          Je bois des Old Fashions dans des noix de coco entre 2 Beach Messes guidées par la foi.
        </p>

        <div class="flex flex-wrap items-center justify-center gap-4 lg:justify-start">
          <BaseButton v-for="section in sections" :key="section.id" :dark-mode="isDarkMode"
            :active="activeSection === section.id" class="hover:border-cyan-400"
            @click="handleSectionClick(section.id)">
            {{ section.id === 'projects' ? 'View Projects' : section.label }}
          </BaseButton>
        </div>
      </div>
    </div>
  </section>
</template>
