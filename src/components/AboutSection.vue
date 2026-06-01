<script setup>
import { nextTick, onMounted, ref } from 'vue'

defineProps({
  isDarkMode: {
    type: Boolean,
    default: false,
  },
})

const levels = [
  { label: 'Quantité de rhum', value: 95, accent: 'bg-cyan-400 text-cyan-400', duration: 850 },
  { label: 'Température', value: 85, accent: 'bg-pink-500 text-pink-500', duration: 1150 },
  { label: 'Amour de mon prochain', value: 100, accent: 'bg-yellow-300 text-yellow-300', duration: 1450 },
]

const animateBars = ref(false)
const aboutHeroImageSrc = `${import.meta.env.BASE_URL}img/aboutheroportrait.png`

const restartBarsAnimation = async () => {
  animateBars.value = false
  await nextTick()
  window.setTimeout(() => {
    animateBars.value = true
  }, 80)
}

onMounted(() => {
  restartBarsAnimation()
})
</script>

<template>
  <section class="relative flex h-full flex-col overflow-hidden py-20 sm:py-28"
    :class="isDarkMode ? 'bg-transparent text-white' : 'bg-transparent text-black'" aria-labelledby="about-title">
    <div
      class="absolute inset-0 z-[1] opacity-5 bg-[radial-gradient(circle,#000_2px,transparent_2px)] [background-size:30px_30px]">
    </div>

    <div class="relative z-10 mx-auto w-full max-w-7xl px-4 sm:px-6 lg:px-8">
      <div class="mb-16 text-center">
        <h2 id="about-title" class="text-4xl font-bold tracking-tight sm:text-5xl lg:text-6xl">
          ABOUT ME
        </h2>
        <div class="mx-auto mt-4 h-2 w-32 bg-pink-500"></div>
      </div>

      <div class="grid items-center gap-12 lg:grid-cols-2 py-10 lg:gap-20">
        <div>
          <p class="mb-6 font-[Inter] text-lg leading-8">
            Je mélange une sensibilité holistique, des ingrédients locaux, et un peu d'huile de coude, pour réaliser des
            cocktails bénis à consommer lors de mes Beach Messes ensoleillées.
          </p>
          <p class="mb-8 font-[Inter] text-lg leading-8">
            Mon travail s'inspire des bikinis colorés, des peaux bronzées, et de ma foi indéfectible envers Notre
            Sauveur
            Jésus Christ de Rio de Janeiro.
          </p>

          <div class="space-y-6">
            <div v-for="level in levels" :key="level.label">
              <div class="mb-2 flex justify-between">
                <span class="font-semibold uppercase">{{ level.label }}</span>
                <span class="font-['Press_Start_2P'] text-[10px]" :class="level.accent.split(' ')[1]">
                  {{ level.value }}%
                </span>
              </div>
              <div class="h-6 overflow-hidden border-4 border-black bg-black"
                :class="isDarkMode ? 'border-cyan-400' : ''">
                <div
                  class="h-full bg-[repeating-linear-gradient(90deg,transparent,transparent_8px,rgba(0,0,0,0.2)_8px,rgba(0,0,0,0.2)_10px)]"
                  :class="level.accent.split(' ')[0]" :style="{
                    width: animateBars ? `${level.value}%` : '0%',
                    transition: `width ${level.duration}ms cubic-bezier(0.16, 1, 0.3, 1)`,
                  }"></div>
              </div>
            </div>
          </div>
        </div>

        <div class="flex justify-center">
          <div class="relative h-[25.5rem] w-[25.5rem] sm:h-[25.5rem] sm:w-[25.5rem]">
            <div class="absolute inset-0 overflow-hidden border-4 border-black bg-cyan-400"
              :class="isDarkMode ? 'border-cyan-400 shadow-[8px_8px_0_0_#22d3ee]' : 'shadow-[8px_8px_0_0_#000]'">
              <img :src="aboutHeroImageSrc" alt="Portrait de presentation" class="h-full w-full object-cover" />
            </div>
            <div class="absolute left-8 top-8 h-full w-full border-4 border-pink-500"></div>
            <div class="absolute left-16 top-16 h-full w-full border-4 border-yellow-300"></div>
            <div v-for="item in ['</>', 'UI', 'API', 'APP']" :key="item"
              class="flex h-16 w-16 items-center justify-center border-4 border-black bg-white text-sm font-bold"
              :class="isDarkMode ? 'bg-zinc-900 text-white border-cyan-400 shadow-[4px_4px_0_0_#22d3ee]' : 'shadow-[4px_4px_0_0_#000]'">
              {{ item }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
