<script setup>
import { computed, nextTick, onBeforeUnmount, onMounted, ref, watch } from 'vue'
import BaseButton from './ui/BaseButton.vue'

const props = defineProps({
  project: {
    type: Object,
    default: null,
  },
  projectIndex: {
    type: Number,
    default: 0,
  },
  displayIndex: {
    type: Number,
    default: 0,
  },
  isDarkMode: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['close'])

const accentMap = {
  cyan: {
    border: 'border-cyan-400',
    text: 'text-cyan-400',
    glow: 'shadow-[0_0_0_2px_#000,0_0_24px_#22d3ee,0_0_42px_rgba(34,211,238,0.45)]',
    badge: 'bg-cyan-400 text-black',
  },
  pink: {
    border: 'border-pink-500',
    text: 'text-pink-500',
    glow: 'shadow-[0_0_0_2px_#000,0_0_24px_#ec4899,0_0_42px_rgba(236,72,153,0.45)]',
    badge: 'bg-pink-500 text-black',
  },
  yellow: {
    border: 'border-yellow-300',
    text: 'text-yellow-300',
    glow: 'shadow-[0_0_0_2px_#000,0_0_24px_#fde047,0_0_42px_rgba(253,224,71,0.45)]',
    badge: 'bg-yellow-300 text-black',
  },
}

const accentOrder = ['cyan', 'pink', 'yellow']
const hasHeroImage = ref(Boolean(props.projectIndex))
const showGlow = ref(false)

const accent = computed(() => {
  return accentMap[accentOrder[props.displayIndex] || 'cyan']
})

const heroImageSrc = computed(() => {
  if (!props.projectIndex) {
    return ''
  }

  return `/img/${props.projectIndex}projectcardportrait.png`
})

const frameGradientStyle = {
  backgroundImage:
    'linear-gradient(120deg, #22d3ee 0%, #ec4899 18%, #fde047 36%, #22d3ee 54%, #ec4899 72%, #fde047 100%)',
  backgroundSize: '400% 400%',
  backgroundPosition: '66.666% 50%',
}

const detailSections = computed(() => {
  if (!props.project) {
    return []
  }

  return [
    {
      label: 'MISSION',
      content: `${props.project.title} met en avant une expérience ${props.project.category?.toLowerCase() || 'interactive'} pensée pour rester lisible, rapide et mémorisable.`,
    },
    {
      label: 'FOCUS',
      content: `Le travail principal porte sur ${props.project.tech?.slice(0, 2).join(' et ') || 'une interface modulaire'} avec une attention particulière à la clarté visuelle et au rythme de navigation.`,
    },
    {
      label: 'RESULT',
      content: `Le rendu assume un ton arcade et une structure one-page pour raconter le projet sans casser la fluidité du portfolio.`,
    },
  ]
})

watch(
  () => props.projectIndex,
  (projectIndex) => {
    hasHeroImage.value = Boolean(projectIndex)
  },
  { immediate: true },
)

const handleHeroImageError = () => {
  hasHeroImage.value = false
}

const handleWindowKeydown = (event) => {
  if (event.key === 'Escape') {
    emit('close')
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleWindowKeydown)
  nextTick(() => {
    requestAnimationFrame(() => {
      showGlow.value = true
    })
  })
})

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleWindowKeydown)
})
</script>

<template>
  <Teleport to="body">
    <div class="fixed inset-0 z-[200] flex items-center justify-center px-4 py-6 sm:px-6 lg:px-8">
      <div class="absolute inset-0 bg-black/30 backdrop-blur-sm" @click="$emit('close')"></div>

      <div class="relative z-10 w-full max-w-6xl">
        <div
          class="pointer-events-none absolute -inset-4 rounded-[1.5rem] blur-3xl transition-all duration-700 ease-out sm:-inset-6"
          :class="[
            showGlow ? 'scale-100 opacity-100' : 'scale-95 opacity-0',
            accent.text === 'text-cyan-400'
              ? 'bg-cyan-400/30'
              : accent.text === 'text-pink-500'
                ? 'bg-pink-500/30'
                : 'bg-yellow-300/30',
          ]"></div>

        <div class="relative flex max-h-[90vh] w-full flex-col overflow-hidden border-4" :class="[
          accent.border,
          accent.glow,
          isDarkMode ? 'bg-zinc-950 text-white' : 'bg-white text-black',
        ]" :style="frameGradientStyle">
          <div class="absolute inset-0 z-0" :class="isDarkMode ? 'bg-zinc-950/74' : 'bg-white/72'"></div>

          <div
            class="pointer-events-none absolute inset-0 z-[1] opacity-10 bg-[repeating-linear-gradient(0deg,transparent,transparent_2px,rgba(255,255,255,0.08)_2px,rgba(255,255,255,0.08)_4px)]">
          </div>

          <div class="relative z-10 flex items-center justify-between gap-4 border-b-4 px-5 py-4 sm:px-8"
            :class="accent.border">
            <div>
              <p class="font-['Press_Start_2P'] text-[10px]" :class="accent.text">
                PROJECT_{{ String(projectIndex).padStart(2, '0') }}
              </p>
              <h3 class="mt-2 text-2xl font-bold uppercase sm:text-3xl">
                {{ project?.title }}
              </h3>
            </div>
            <BaseButton compact :dark-mode="isDarkMode" @click="$emit('close')">
              Close
            </BaseButton>
          </div>

          <div class="relative z-10 grid overflow-y-auto lg:grid-cols-[1.2fr_0.9fr]">
            <div class="relative min-h-[320px] border-b-4 lg:border-b-0 lg:border-r-4" :class="accent.border">
              <img v-if="hasHeroImage" :src="heroImageSrc" alt="Illustration du projet"
                class="absolute inset-0 h-full w-full object-cover" @error="handleHeroImageError" />
              <div v-else
                class="absolute inset-0 bg-[radial-gradient(circle_at_top,_rgba(34,211,238,0.35),_transparent_55%),linear-gradient(135deg,rgba(236,72,153,0.18),transparent_42%),linear-gradient(180deg,rgba(0,0,0,0.22),rgba(0,0,0,0.55))]">
              </div>
              <div class="absolute inset-0 bg-black/35"></div>
              <div
                class="pointer-events-none absolute inset-0 bg-[repeating-linear-gradient(90deg,transparent,transparent_16px,rgba(255,255,255,0.08)_16px,rgba(255,255,255,0.08)_17px)]">
              </div>

              <div class="relative z-10 flex h-full flex-col justify-end gap-4 p-6 sm:p-8">
                <span class="inline-flex w-fit border-4 px-3 py-2 font-['Press_Start_2P'] text-[10px]"
                  :class="[accent.badge, accent.border]">
                  {{ project?.category || 'PROJECT' }}
                </span>
                <p class="max-w-xl font-[Inter] text-sm leading-7 text-white/90 sm:text-base">
                  {{ project?.description }}
                </p>
              </div>
            </div>

            <div class="relative p-6 sm:p-8">
              <div class="pointer-events-none absolute inset-0 opacity-35"
                :class="isDarkMode ? 'bg-zinc-950/30' : 'bg-white/35'"></div>
              <div class="mb-8 flex flex-wrap gap-3">
                <span v-for="item in project?.tech || []" :key="item"
                  class="relative z-10 border-4 bg-black px-3 py-2 font-['Press_Start_2P'] text-[10px]"
                  :class="[accent.text, accent.border]">
                  {{ item }}
                </span>
              </div>

              <div class="relative z-10 space-y-6">
                <div v-for="section in detailSections" :key="section.label" class="border-4  bg-black/10 p-4"
                  :class="accent.border">
                  <p class="font-['Press_Start_2P'] text-[10px]" :class="accent.text">
                    {{ section.label }}
                  </p>
                  <p class="mt-3 font-[Inter] text-sm leading-7 opacity-90">
                    {{ section.content }}
                  </p>
                </div>
              </div>

              <div class="relative z-10 mt-8 flex flex-wrap items-center gap-4">
                <BaseButton compact :dark-mode="isDarkMode" @click="$emit('close')">
                  Back To Grid
                </BaseButton>
                <p class="font-['Press_Start_2P'] text-[10px]" :class="accent.text">
                  PRESS ESC TO CLOSE
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Teleport>
</template>
