<script setup>
import { computed, ref, watch } from 'vue'
import ProjectDetail from './ProjectDetail.vue'
import BaseButton from './ui/BaseButton.vue'
import ProjectCard from './ui/ProjectCard.vue'

defineProps({
  isDarkMode: {
    type: Boolean,
    default: false,
  },
})

const projects = [
  {
    id: 1,
    title: 'Pèlerinage immersif',
    description: 'Voyage introspectif de club en club.',
    category: 'Frontend',
    tech: ['Humilité', 'Prière', 'Disco-Ball'],
  },
  {
    id: 2,
    title: 'Danse rituelle',
    description: 'Cérémonie contemplative de mon corps huilé.',
    category: 'Frontend',
    tech: ['Capoeira', 'muscles saillants', "Élévation de l'âme"],
  },
  {
    id: 3,
    title: 'Sunset Baptême',
    description: 'Se laisser pénétrer par les voies du Seigneur.',
    category: 'Frontend',
    tech: ['Chapelet', 'Latin', 'Communion'],
  },
  {
    id: 4,
    title: 'Retro Booking',
    description: 'Parcours de reservation web au style arcade avec filtres et transitions fluides.',
    category: 'Frontend',
    tech: ['Vue', 'Router', 'UX'],
  },
  {
    id: 5,
    title: 'Pixel Admin',
    description: 'Back office visuel pour gerer contenu, medias et etats de publication.',
    category: 'Frontend',
    tech: ['Vue', 'Forms', 'API'],
  },
  {
    id: 6,
    title: 'Motion Catalog',
    description: 'Catalogue de composants anime avec recherche instantanee et preview live.',
    category: 'Frontend',
    tech: ['Vue', 'Storybook', 'Vite'],
  },
  {
    id: 7,
    title: 'Arcade Store',
    description: 'Boutique e-commerce avec identite visuelle pixel et navigation rapide.',
    category: 'Full Stack',
    tech: ['Vue', 'Stripe', 'Mongo'],
  },
  {
    id: 8,
    title: 'Synth Market',
    description: 'Marketplace de produits numeriques avec tunnel de vente et espace vendeur.',
    category: 'Full Stack',
    tech: ['Vue', 'Node', 'Postgres'],
  },
  {
    id: 9,
    title: 'Level Manager',
    description: 'Plateforme de gestion d equipe avec authentification, roles et tableaux de bord.',
    category: 'Full Stack',
    tech: ['Vue', 'Express', 'MySQL'],
  },
  {
    id: 10,
    title: 'Neon Tickets',
    description: 'Systeme de reservation evenementielle avec paiement et billets dynamiques.',
    category: 'Full Stack',
    tech: ['Vue', 'Stripe', 'Prisma'],
  },
  {
    id: 11,
    title: 'Drop Zone',
    description: 'Outil de partage de fichiers securise avec espace client et suivi d uploads.',
    category: 'Full Stack',
    tech: ['Vue', 'Laravel', 'S3'],
  },
  {
    id: 12,
    title: 'Quest Planner',
    description: 'Application collaborative de planification avec commentaires et jalons projet.',
    category: 'Full Stack',
    tech: ['Vue', 'Socket', 'Mongo'],
  },
  {
    id: 13,
    title: 'Pixel Chat',
    description: 'Messagerie temps reel avec ambiance borne d arcade.',
    category: 'Backend',
    tech: ['Socket', 'Redis', 'Node'],
  },
  {
    id: 14,
    title: 'Pulse API',
    description: 'API de notifications temps reel avec files de traitement et webhooks.',
    category: 'Backend',
    tech: ['Node', 'Redis', 'Queue'],
  },
  {
    id: 15,
    title: 'Vault Auth',
    description: 'Service d authentification centralise avec sessions, tokens et audit trail.',
    category: 'Backend',
    tech: ['Node', 'JWT', 'Postgres'],
  },
  {
    id: 16,
    title: 'Echo Stream',
    description: 'Pipeline de diffusion de donnees avec traitement d evenements et monitoring.',
    category: 'Backend',
    tech: ['Kafka', 'Nest', 'Grafana'],
  },
  {
    id: 17,
    title: 'Core Inventory',
    description: 'Moteur de stock et disponibilite expose via API pour plusieurs frontends.',
    category: 'Backend',
    tech: ['Fastify', 'SQL', 'Docker'],
  },
  {
    id: 18,
    title: 'Arena Sync',
    description: 'Service de synchronisation multi-sources pour consolidations metier nocturnes.',
    category: 'Backend',
    tech: ['Python', 'Workers', 'Postgres'],
  },
]

const selectedCategory = ref('all')
const currentPage = ref(1)
const cardsPerPage = 3
const selectedProjectEntry = ref(null)

const categories = computed(() => {
  const projectCategories = projects.map((project) => project.category).filter(Boolean)
  return [...new Set(projectCategories)]
})

const filteredProjects = computed(() => {
  if (selectedCategory.value === 'all') {
    return projects
  }

  return projects.filter((project) => project.category === selectedCategory.value)
})

const totalPages = computed(() => {
  return Math.max(1, Math.ceil(filteredProjects.value.length / cardsPerPage))
})

const paginatedProjects = computed(() => {
  const start = (currentPage.value - 1) * cardsPerPage
  return filteredProjects.value.slice(start, start + cardsPerPage).map((project) => ({
    project,
    projectIndex: projects.indexOf(project) + 1,
  }))
})

const goToPage = (page) => {
  currentPage.value = page
}

const goToPreviousPage = () => {
  if (currentPage.value > 1) {
    currentPage.value -= 1
  }
}

const goToNextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value += 1
  }
}

const openProjectDetail = (projectEntry, displayIndex) => {
  selectedProjectEntry.value = {
    ...projectEntry,
    displayIndex,
  }
}

const closeProjectDetail = () => {
  selectedProjectEntry.value = null
}

watch(selectedCategory, () => {
  currentPage.value = 1
  closeProjectDetail()
})
</script>

<template>
  <section class="relative flex h-full flex-col py-20 sm:py-28"
    :class="isDarkMode ? 'bg-transparent text-white' : 'bg-transparent text-black'" aria-labelledby="projects-title">
    <div
      class="absolute inset-0 z-[1] opacity-5 bg-[repeating-linear-gradient(45deg,transparent,transparent_10px,#000_10px,#000_11px)]">
    </div>

    <div class="relative z-10 mx-auto w-full max-w-7xl px-4 sm:px-6 lg:px-8">
      <div class="text-center">
        <h2 id="projects-title" class="text-4xl font-bold tracking-tight sm:text-5xl lg:text-6xl">
          PROJECTS
        </h2>
        <div class="mx-auto mt-4 h-2 w-32 bg-cyan-400"></div>
      </div>

      <div class="mb-10 flex flex-col items-center justify-between gap-4 md:flex-row">
        <div class="font-['Press_Start_2P'] text-[10px] text-cyan-400 sm:text-xs">FILTER PROJECTS</div>
        <label class="flex items-center gap-3">
          <span class="font-['Press_Start_2P'] text-[10px] sm:text-xs">CATEGORY</span>
          <select v-model="selectedCategory"
            class="min-w-48 border-4 border-black bg-white px-4 py-3 text-sm font-semibold text-black outline-none transition focus:border-cyan-400"
            :class="isDarkMode ? 'border-cyan-400 bg-zinc-900 text-white' : ''">
            <option value="all">Toutes</option>
            <option v-for="category in categories" :key="category" :value="category">
              {{ category }}
            </option>
          </select>
        </label>
      </div>

      <div v-if="paginatedProjects.length" class="grid grid-cols-1 gap-8 xl:grid-cols-3">
        <ProjectCard v-for="({ project, projectIndex }, index) in paginatedProjects" :key="project.id || project.title"
          :project="project" :project-index="projectIndex" :display-index="index" :is-dark-mode="isDarkMode"
          @open="openProjectDetail({ project, projectIndex }, index)" />
      </div>

      <div v-if="totalPages > 1" class="mt-10 flex flex-col items-center gap-4">
        <p class="font-['Press_Start_2P'] text-[10px] text-cyan-400 sm:text-xs">
          PAGE {{ currentPage }} / {{ totalPages }}
        </p>
        <div class="flex flex-wrap items-center justify-center gap-3">
          <BaseButton compact :dark-mode="isDarkMode" :class="currentPage === 1 ? 'pointer-events-none opacity-50' : ''"
            @click="goToPreviousPage">
            Prev
          </BaseButton>
          <BaseButton v-for="page in totalPages" :key="page" compact :dark-mode="isDarkMode"
            :active="currentPage === page" @click="goToPage(page)">
            {{ page }}
          </BaseButton>
          <BaseButton compact :dark-mode="isDarkMode"
            :class="currentPage === totalPages ? 'pointer-events-none opacity-50' : ''" @click="goToNextPage">
            Next
          </BaseButton>
        </div>
      </div>

      <div v-else class="flex min-h-60 items-center justify-center border-4 border-dashed border-black text-center"
        :class="isDarkMode ? 'border-cyan-400' : ''">
        <p class="font-['Press_Start_2P'] text-xs text-pink-500">NO PROJECTS IN THIS CATEGORY</p>
      </div>
    </div>

    <Transition enter-active-class="transition duration-500 ease-out" enter-from-class="opacity-0 translate-y-6"
      enter-to-class="opacity-100 translate-y-0" leave-active-class="transition duration-400 ease-in"
      leave-from-class="opacity-100 translate-y-0" leave-to-class="opacity-0 translate-y-6">
      <ProjectDetail v-if="selectedProjectEntry" :project="selectedProjectEntry.project"
        :project-index="selectedProjectEntry.projectIndex" :display-index="selectedProjectEntry.displayIndex"
        :is-dark-mode="isDarkMode" @close="closeProjectDetail" />
    </Transition>
  </section>
</template>
