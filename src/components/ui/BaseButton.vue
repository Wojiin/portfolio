<script setup>
defineProps({
  type: {
    type: String,
    default: 'button',
  },
  darkMode: {
    type: Boolean,
    default: false,
  },
  compact: {
    type: Boolean,
    default: false,
  },
  block: {
    type: Boolean,
    default: false,
  },
  active: {
    type: Boolean,
    default: false,
  },
})

defineEmits(['click'])
</script>

<template>
  <button
    :type="type"
    class="cursor-pointer border-4 bg-cyan-400 font-bold uppercase tracking-wide text-black transition duration-200 hover:translate-x-1 hover:translate-y-1 hover:shadow-[0_0_0_2px_#000,0_0_18px_#22d3ee,0_0_34px_#ec4899] focus-visible:translate-x-1 focus-visible:translate-y-1 focus-visible:outline-4 focus-visible:outline-offset-4 focus-visible:outline-yellow-300"
    :class="[
      active
        ? 'border-cyan-400 text-cyan-400 bg-black shadow-[0_0_0_2px_#000,0_0_18px_#22d3ee,0_0_34px_#ec4899] [text-shadow:0_0_8px_#22d3ee,0_0_18px_#22d3ee,2px_2px_0_#ec4899]'
        : darkMode
          ? 'border-cyan-400 shadow-[6px_6px_0_0_#000]'
          : 'border-black shadow-[6px_6px_0_0_#000]',
      compact ? 'px-4 py-2 text-[10px]' : 'px-6 py-3 text-xs sm:px-8 sm:py-4 sm:text-sm',
      block ? 'w-full' : 'inline-block',
    ]"
    @click="$emit('click', $event)"
  >
    <slot />
  </button>
</template>
