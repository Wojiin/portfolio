<script setup>
import { computed, ref, watch } from 'vue'

const props = defineProps({
  project: {
    type: Object,
    default: () => ({}),
  },
  displayIndex: {
    type: Number,
    default: 0,
  },
  projectIndex: {
    type: Number,
    default: 0,
  },
  isDarkMode: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['open'])

const accentMap = {
  cyan: {
    border: 'border-cyan-400',
    text: 'text-cyan-400',
    surface: 'bg-gradient-to-br from-cyan-400/18 to-pink-500/18',
    shadow: 'shadow-[8px_8px_0_0_#000]',
    hoverGlow: 'hover:shadow-[12px_12px_0_#000,0_0_18px_#22d3ee,0_0_34px_#22d3ee] focus-visible:shadow-[12px_12px_0_#000,0_0_18px_#22d3ee,0_0_34px_#22d3ee]',
  },
  pink: {
    border: 'border-pink-500',
    text: 'text-pink-500',
    surface: 'bg-gradient-to-br from-yellow-300/20 to-pink-500/18',
    shadow: 'shadow-[8px_8px_0_0_#000]',
    hoverGlow: 'hover:shadow-[12px_12px_0_#000,0_0_18px_#ec4899,0_0_34px_#ec4899] focus-visible:shadow-[12px_12px_0_#000,0_0_18px_#ec4899,0_0_34px_#ec4899]',
  },
  yellow: {
    border: 'border-yellow-300',
    text: 'text-yellow-300',
    surface: 'bg-gradient-to-br from-cyan-400/18 to-yellow-300/20',
    shadow: 'shadow-[8px_8px_0_0_#000]',
    hoverGlow: 'hover:shadow-[12px_12px_0_#000,0_0_18px_#fde047,0_0_34px_#fde047] focus-visible:shadow-[12px_12px_0_#000,0_0_18px_#fde047,0_0_34px_#fde047]',
  },
}

const accentOrder = ['cyan', 'pink', 'yellow']
const accent = accentMap[accentOrder[props.displayIndex] || 'cyan']
const hasHeaderImage = ref(Boolean(props.projectIndex))

const headerImageSrc = computed(() => {
  if (!props.projectIndex) {
    return ''
  }

  return `/img/${props.projectIndex}projectcardportrait.png`
})

watch(
  () => props.projectIndex,
  (projectIndex) => {
    hasHeaderImage.value = Boolean(projectIndex)
  },
  { immediate: true },
)

const handleHeaderImageError = () => {
  hasHeaderImage.value = false
}
</script>

<template>
  <article
    class="group flex h-full flex-col overflow-hidden border-4 transition duration-300 hover:-translate-y-2 focus-visible:-translate-y-2 focus-visible:outline-4 focus-visible:outline-offset-4 focus-visible:outline-yellow-300"
    :class="[
      accent.border,
      accent.shadow,
      accent.hoverGlow,
      isDarkMode ? 'bg-zinc-900' : 'bg-white',
    ]" tabindex="0" role="button" @click="emit('open')" @keydown.enter.prevent="emit('open')"
    @keydown.space.prevent="emit('open')">
    <div class="relative h-48 overflow-hidden border-b-4 bg-black/5" :class="accent.border">
      <img v-if="hasHeaderImage" :src="headerImageSrc" alt="Illustration du projet"
        class="absolute inset-0 h-full w-full object-cover" @error="handleHeaderImageError" />
      <div v-if="hasHeaderImage" class="absolute inset-0 bg-black/30"></div>
      <div
        class="pointer-events-none absolute inset-0 bg-[repeating-linear-gradient(0deg,rgba(0,0,0,0.08)_0,rgba(0,0,0,0.08)_1px,transparent_1px,transparent_2px)]">
      </div>
      <div class="absolute inset-0 flex items-center justify-center">
        <span class="px-4 text-center font-['Press_Start_2P'] text-sm leading-6" :class="accent.text">
          {{ project.category || 'PROJECT' }}
        </span>
      </div>
    </div>

    <div class="flex flex-1 flex-col p-6" :class="accent.surface">
      <h3 class="mb-2 border-b-4 pb-2 text-xl font-bold uppercase" :class="accent.border">
        {{ project.title || 'Projet' }}
      </h3>
      <p class="mb-4 flex-1 font-[Inter] text-sm leading-6 opacity-90">
        {{ project.description || '' }}
      </p>
      <div class="flex flex-wrap gap-2">
        <span v-for="item in project.tech || []" :key="item"
          class="bg-black px-3 py-1 font-['Press_Start_2P'] text-[10px] transition duration-150 group-hover:-translate-y-0.5 group-hover:shadow-[0_0_10px_currentColor,0_0_18px_currentColor] group-focus-visible:-translate-y-0.5 group-focus-visible:shadow-[0_0_10px_currentColor,0_0_18px_currentColor]"
          :class="accent.text">
          {{ item }}
        </span>
      </div>
    </div>
  </article>
</template>
