<script setup>
defineProps({
  darkMode: {
    type: Boolean,
    default: false,
  },
})

defineEmits(['toggle'])
</script>

<template>
  <button
    type="button"
    class="relative h-8 w-16 cursor-pointer overflow-hidden border-4 transition duration-200 hover:shadow-[0_0_0_2px_#000,0_0_18px_#22d3ee,0_0_30px_#fde047] focus-visible:outline-4 focus-visible:outline-offset-4 focus-visible:outline-pink-500"
    :class="darkMode ? 'border-cyan-400 bg-zinc-900' : 'border-black bg-black'"
    @click="$emit('toggle')"
  >
    <div
      class="absolute top-0 flex h-6 w-6 items-center justify-center bg-cyan-400 transition-transform duration-300"
      :class="darkMode ? 'translate-x-6' : 'translate-x-0'"
    >
      <slot name="thumb">{{ darkMode ? '☾' : '☀' }}</slot>
    </div>
  </button>
</template>
