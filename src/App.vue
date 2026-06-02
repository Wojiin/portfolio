<script setup>
import { computed, ref } from 'vue'
import AboutSection from './components/AboutSection.vue'
import ContactForm from './components/ContactForm.vue'
import HomeSection from './components/HomeSection.vue'
import Navigation from './components/Navigation.vue'
import ProjectList from './components/ProjectList.vue'
import Skills from './components/Skills.vue'
import BaseButton from './components/ui/BaseButton.vue'

const isDarkMode = ref(false)
const activeSectionId = ref('home')
const backgroundStep = ref(0)

const sections = [
  { id: 'home', label: 'Home', component: HomeSection },
  { id: 'about', label: 'About', component: AboutSection },
  { id: 'projects', label: 'Projects', component: ProjectList },
  { id: 'skills', label: 'Skills', component: Skills },
  { id: 'contact', label: 'Contact', component: ContactForm },
]

const heroSections = sections.filter((section) => section.id !== 'home')

const currentSection = computed(() => {
  return sections.find((section) => section.id === activeSectionId.value)
})

const usesSharedOverlay = computed(() => {
  return ['home', 'projects', 'about', 'skills'].includes(activeSectionId.value)
})

const gradientPosition = computed(() => {
  const positions = ['0% 50%', '33.333% 50%', '66.666% 50%', '100% 50%']
  return positions[backgroundStep.value]
})

const frameGradientStyle = computed(() => {
  return {
    backgroundImage:
      'linear-gradient(120deg, #22d3ee 0%, #ec4899 18%, #fde047 36%, #22d3ee 54%, #ec4899 72%, #fde047 100%)',
    backgroundSize: '400% 400%',
    backgroundPosition: gradientPosition.value,
  }
})

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value
}

const incrementBackgroundStep = () => {
  backgroundStep.value = (backgroundStep.value + 1) % 4
}

const handleSectionNavigation = (sectionId) => {
  if (sectionId !== activeSectionId.value) {
    incrementBackgroundStep()
  }

  activeSectionId.value = sectionId
}
</script>

<template>
  <div :class="[
    'flex min-h-screen flex-col font-[Space_Grotesk] transition-colors duration-500',
    isDarkMode ? 'bg-zinc-950 text-white' : 'bg-white text-black',
  ]">
    <div class="pointer-events-none fixed inset-0 z-0 transition-[background-position,opacity] duration-700 ease-out"
      :class="activeSectionId === 'contact' ? 'opacity-0' : 'opacity-100'" :style="{
        backgroundImage:
          'linear-gradient(120deg, #22d3ee 0%, #ec4899 18%, #fde047 36%, #22d3ee 54%, #ec4899 72%, #fde047 100%)',
        backgroundSize: '400% 400%',
        backgroundPosition: gradientPosition,
      }"></div>

    <div class="pointer-events-none fixed inset-0 z-10 opacity-10" :class="[
      isDarkMode
        ? 'bg-[repeating-linear-gradient(0deg,transparent,transparent_2px,rgba(34,211,238,0.08)_2px,rgba(34,211,238,0.08)_4px)]'
        : 'bg-[repeating-linear-gradient(0deg,transparent,transparent_2px,rgba(0,0,0,0.1)_2px,rgba(0,0,0,0.1)_4px)]',
    ]"></div>

    <Navigation :sections="sections" :active-section="activeSectionId" :is-dark-mode="isDarkMode"
      :frame-gradient-style="frameGradientStyle" @select-section="handleSectionNavigation"
      @toggle-theme="toggleTheme" />

    <main class="relative z-20 flex flex-1 overflow-hidden">
      <div v-if="usesSharedOverlay" class="pointer-events-none absolute inset-0 z-0 bg-white/72 backdrop-blur-[2px]"
        :class="isDarkMode ? 'bg-zinc-950/74' : 'bg-white/72'"></div>
      <div v-if="activeSectionId === 'home'"
        class="pointer-events-none absolute inset-0 z-[1] opacity-10 bg-[radial-gradient(circle,#000_1px,transparent_1px)] [background-size:20px_20px]">
      </div>
      <Transition mode="out-in" enter-active-class="transition duration-700 ease-out"
        enter-from-class="translate-x-10 opacity-0" enter-to-class="translate-x-0 opacity-100"
        leave-active-class="transition duration-700 ease-out" leave-from-class="translate-x-0 opacity-100"
        leave-to-class="-translate-x-10 opacity-0">
        <component :is="currentSection.component" :key="activeSectionId" class="relative z-10 flex-1"
          :is-dark-mode="isDarkMode" :sections="currentSection.id === 'home' ? heroSections : undefined"
          :active-section="activeSectionId" @select-section="handleSectionNavigation" />
      </Transition>
    </main>

    <footer class="relative shrink-0 py-8 transition-colors duration-500" style="z-index: 20"
      :class="isDarkMode ? 'bg-zinc-950' : 'bg-white'">
      <div class="absolute inset-x-0 top-0 h-1 transition-[background-position] duration-700 ease-out"
        :style="frameGradientStyle"></div>
      <div
        class="absolute inset-0 opacity-5 bg-[repeating-linear-gradient(90deg,transparent,transparent_20px,#000_20px,#000_21px)]">
      </div>
      <div
        class="relative mx-auto flex max-w-7xl flex-col items-center justify-between gap-4 px-4 sm:flex-row sm:px-6 lg:px-8">
        <p class="text-center font-['Press_Start_2P'] text-[10px] sm:text-left">
          © 2026 Wojiin.PORTFOLIO // ALL RIGHTS RESERVED ( si tu shake ton booty je te fais une réduc')
        </p>
        <BaseButton compact :dark-mode="isDarkMode" class="group flex items-center gap-2"
          @click="handleSectionNavigation('home')">
          <span class="font-['Press_Start_2P'] text-[10px]">Back To Home</span>
        </BaseButton>
      </div>
    </footer>
  </div>
</template>
