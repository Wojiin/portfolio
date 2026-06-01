<script setup>
defineProps({
  active: {
    type: Boolean,
    default: false,
  },
  mobile: {
    type: Boolean,
    default: false,
  },
})

defineEmits(['click'])
</script>

<template>
  <button
    type="button"
    class="group relative cursor-pointer border-b-4 transition duration-300 hover:-translate-y-0.5 hover:border-cyan-400 hover:text-cyan-400 hover:[text-shadow:0_0_6px_#22d3ee,0_0_16px_#22d3ee,2px_2px_0_#ec4899] focus-visible:outline-4 focus-visible:outline-offset-4 focus-visible:outline-yellow-300"
    :class="[
      mobile ? 'block w-full py-2 text-left text-lg font-semibold' : 'pb-1 text-lg font-semibold xl:text-xl',
      active
        ? 'border-cyan-400 text-cyan-400 [text-shadow:0_0_8px_#22d3ee,0_0_18px_#22d3ee,2px_2px_0_#ec4899]'
        : 'border-transparent',
    ]"
    @click="$emit('click', $event)"
  >
    <slot />
    <span
      v-if="!active"
      class="absolute -bottom-[0.45rem] left-0 h-[0.45rem] w-full origin-left scale-x-0 bg-gradient-to-r from-cyan-400 via-pink-500 to-yellow-300 opacity-90 shadow-[0_0_10px_#22d3ee,0_0_18px_#ec4899] transition-transform duration-300 ease-out group-hover:scale-x-100"
    ></span>
    <span
      v-else
      class="absolute -bottom-[0.45rem] left-1/2 h-[0.45rem] w-full -translate-x-1/2 animate-pulse bg-gradient-to-r from-cyan-400 via-pink-500 to-yellow-300 shadow-[0_0_10px_#22d3ee,0_0_18px_#ec4899]"
    ></span>
  </button>
</template>
