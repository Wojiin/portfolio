<script setup>
import { reactive, ref, watch } from 'vue'
import BaseButton from './ui/BaseButton.vue'

defineProps({
  isDarkMode: {
    type: Boolean,
    default: false,
  },
})

const form = reactive({
  name: '',
  email: '',
  message: '',
})

const submissionCount = ref(0)
const statusMessage = ref('> Prêt à recevoir vos doléances...')
const statusTone = ref('text-cyan-400')

watch(submissionCount, (count) => {
  if (count > 0) {
    statusMessage.value = '> Votre prière a bien été envoyée.'
    statusTone.value = 'text-yellow-300'
  }
})

const handleSubmit = () => {
  submissionCount.value += 1
  form.name = ''
  form.email = ''
  form.message = ''
}
</script>

<template>
  <section class="relative isolate flex min-h-full w-full flex-col justify-center overflow-hidden bg-black text-white">
    <div
      class="pointer-events-none absolute inset-0 opacity-10 bg-[repeating-linear-gradient(0deg,transparent,transparent_2px,#22d3ee_2px,#22d3ee_4px)]">
    </div>

    <div class="relative z-10 mx-auto flex w-full max-w-7xl flex-col justify-center px-4 sm:px-6 lg:px-8">
      <div class="mb-16 text-center">
        <h2 id="contact-title" class="text-4xl font-bold tracking-tight sm:text-5xl lg:text-6xl">
          CONTACT
        </h2>
        <div class="mx-auto mt-4 h-2 w-32 bg-pink-500"></div>
      </div>

      <div class="mx-auto w-full max-w-2xl">
        <div class="relative">
          <div
            class="pointer-events-none absolute -inset-6 animate-pulse rounded-[0.75rem] bg-cyan-400/30 blur-3xl [animation-duration:2.8s]">
          </div>
          <div
            class="pointer-events-none absolute -inset-10 animate-ping rounded-[1rem] bg-pink-500/15 blur-[72px] [animation-duration:2.8s]">
          </div>
          <div
            class="relative border-4 border-cyan-400 bg-black p-6 shadow-[8px_8px_0_0_#22d3ee,0_0_30px_rgba(34,211,238,0.45)] sm:p-8">
            <div class="mb-6 flex items-center gap-2 border-b-2 border-cyan-400 pb-4">
              <div class="h-3 w-3 rounded-full bg-pink-500"></div>
              <div class="h-3 w-3 rounded-full bg-yellow-300"></div>
              <div class="h-3 w-3 rounded-full bg-cyan-400"></div>
              <span class="ml-4 font-['Press_Start_2P'] text-[10px] text-cyan-400">CONTACT.EXE</span>
            </div>

            <form class="space-y-6" @submit.prevent="handleSubmit">
              <label class="block">
                <span class="mb-2 block font-['Press_Start_2P'] text-[10px] text-cyan-400">NAME_</span>
                <input v-model="form.name" type="text" name="name" placeholder="Enter your name..."
                  class="w-full border-4 border-cyan-400 bg-transparent px-4 py-3 font-[Inter] text-white outline-none transition duration-150 hover:border-yellow-300 hover:shadow-[inset_0_0_0_2px_rgba(0,0,0,0.35),0_0_12px_rgba(253,224,71,0.5)] focus:translate-x-0.5 focus:border-pink-500 focus:bg-white/5 focus:shadow-[0_0_0_2px_#000,0_0_0_6px_#22d3ee,0_0_22px_#ec4899] focus:placeholder:text-yellow-300" />
              </label>

              <label class="block">
                <span class="mb-2 block font-['Press_Start_2P'] text-[10px] text-cyan-400">EMAIL_</span>
                <input v-model="form.email" type="email" name="email" placeholder="Enter your email..."
                  class="w-full border-4 border-cyan-400 bg-transparent px-4 py-3 font-[Inter] text-white outline-none transition duration-150 hover:border-yellow-300 hover:shadow-[inset_0_0_0_2px_rgba(0,0,0,0.35),0_0_12px_rgba(253,224,71,0.5)] focus:translate-x-0.5 focus:border-pink-500 focus:bg-white/5 focus:shadow-[0_0_0_2px_#000,0_0_0_6px_#22d3ee,0_0_22px_#ec4899] focus:placeholder:text-yellow-300" />
              </label>

              <label class="block">
                <span class="mb-2 block font-['Press_Start_2P'] text-[10px] text-cyan-400">MESSAGE_</span>
                <textarea v-model="form.message" name="message" rows="5" placeholder="Type your message..."
                  class="w-full resize-none border-4 border-cyan-400 bg-transparent px-4 py-3 font-[Inter] text-white outline-none transition duration-150 hover:border-yellow-300 hover:shadow-[inset_0_0_0_2px_rgba(0,0,0,0.35),0_0_12px_rgba(253,224,71,0.5)] focus:translate-x-0.5 focus:border-pink-500 focus:bg-white/5 focus:shadow-[0_0_0_2px_#000,0_0_0_6px_#22d3ee,0_0_22px_#ec4899] focus:placeholder:text-yellow-300"></textarea>
              </label>

              <BaseButton type="submit" block dark-mode class="hover:border-pink-500 hover:bg-pink-500">
                SEND MESSAGE
              </BaseButton>
            </form>

            <div class="mt-6 border-t-2 border-cyan-400 pt-4">
              <p class="font-['Press_Start_2P'] text-[10px]" :class="statusTone">{{ statusMessage }}</p>
              <p class="mt-1 font-['Press_Start_2P'] text-[10px]" :class="statusTone">><span
                  class="animate-pulse">_</span></p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
