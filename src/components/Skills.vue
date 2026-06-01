<script setup>
import SkillBadge from './SkillBadge.vue'

defineProps({
  isDarkMode: {
    type: Boolean,
    default: false,
  },
})

const skills = [
  { id: 1, name: 'Frontend', level: 95, accent: 'cyan' },
  { id: 2, name: 'Backend', level: 85, accent: 'pink' },
  { id: 3, name: 'UI / UX', level: 90, accent: 'yellow' },
  { id: 4, name: 'Vue.js', level: 92, accent: 'cyan' },
  { id: 5, name: 'Tailwind', level: 88, accent: 'pink' },
  { id: 6, name: 'Design System', level: 84, accent: 'yellow' },
]
</script>

<template>
  <section
    class="relative flex h-full flex-col py-20 sm:py-28"
    :class="isDarkMode ? 'bg-transparent text-white' : 'bg-transparent text-black'"
    aria-labelledby="skills-title"
  >
    <div class="relative z-10 mx-auto flex w-full max-w-7xl flex-1 flex-col px-4 sm:px-6 lg:px-8">
      <div class="mb-16 text-center">
        <h2 id="skills-title" class="text-4xl font-bold tracking-tight sm:text-5xl lg:text-6xl">
          SKILLS
        </h2>
        <div class="mx-auto mt-4 h-2 w-32 bg-yellow-300"></div>
      </div>

      <div v-if="skills.length" class="grid gap-4 md:grid-cols-2 xl:grid-cols-3">
        <SkillBadge
          v-for="(skill, index) in skills"
          :key="skill.id || skill.name"
          :skill="skill"
          :index="index"
          :is-dark-mode="isDarkMode"
        />
      </div>

      <div
        v-else
        class="flex min-h-40 items-center justify-center border-4 border-dashed border-black"
        :class="isDarkMode ? 'border-cyan-400' : ''"
      >
        <span class="font-['Press_Start_2P'] text-xs text-cyan-400">NO SKILLS YET</span>
      </div>
    </div>
  </section>
</template>
