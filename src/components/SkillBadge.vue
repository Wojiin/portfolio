<script setup>
import { nextTick, onMounted, ref } from 'vue'

const props = defineProps({
  skill: {
    type: Object,
    default: () => ({}),
  },
  index: {
    type: Number,
    default: 0,
  },
  isDarkMode: {
    type: Boolean,
    default: false,
  },
})

const accentMap = {
  cyan: 'bg-cyan-400 text-black',
  pink: 'bg-pink-500 text-black',
  yellow: 'bg-yellow-300 text-black',
}

const accentClass = accentMap[props.skill.accent] || accentMap.cyan
const animateBar = ref(false)

const animationDuration = 700 + props.index * 140

const restartBarAnimation = async () => {
  animateBar.value = false
  await nextTick()
  window.setTimeout(() => {
    animateBar.value = true
  }, 30 + props.index * 20)
}

onMounted(() => {
  restartBarAnimation()
})
</script>

<template>
  <article
    class="border-4 p-5 transition hover:-translate-y-1"
    :class="[
      isDarkMode ? 'border-cyan-400 bg-zinc-900 shadow-[6px_6px_0_0_#000]' : 'border-black bg-white shadow-[6px_6px_0_0_#000]',
    ]"
  >
    <div class="mb-4 flex items-center justify-between gap-4">
      <span class="font-['Press_Start_2P'] text-[10px] sm:text-xs">{{ skill.name || 'Competence' }}</span>
      <span class="px-2 py-1 font-['Press_Start_2P'] text-[10px]" :class="accentClass">{{ skill.level || 0 }}%</span>
    </div>
    <div class="h-5 overflow-hidden border-4 border-black bg-black" :class="isDarkMode ? 'border-cyan-400' : ''">
      <div
        class="h-full bg-[repeating-linear-gradient(90deg,transparent,transparent_8px,rgba(0,0,0,0.2)_8px,rgba(0,0,0,0.2)_10px)]"
        :class="accentClass.split(' ')[0]"
        :style="{
          width: animateBar ? `${skill.level || 0}%` : '0%',
          transition: `width ${animationDuration}ms cubic-bezier(0.16, 1, 0.3, 1)`,
        }"
      ></div>
    </div>
  </article>
</template>
