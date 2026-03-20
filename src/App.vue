<template>
  <div id="top" class="min-h-screen bg-gray-50 dark:bg-gray-900 transition-colors duration-300">
    <!-- Dark Mode Toggle -->
    <div class="fixed top-4 right-4 z-50">
      <button 
        @click="toggleDarkMode" 
        class="p-3 bg-white dark:bg-gray-800 rounded-full shadow-lg hover:shadow-xl transition-all duration-300 border border-gray-200 dark:border-gray-700"
        :class="{ flip: flipActive }"
        aria-label="Alternar modo escuro"
        title="Alternar modo escuro"
      >
        <svg v-if="!isDark" class="w-5 h-5 text-gray-600" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z" clip-rule="evenodd"></path>
        </svg>
        <svg v-else class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
          <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"></path>
        </svg>
      </button>
    </div>

    <!-- Header/Hero Section -->
    <header class="bg-white dark:bg-gray-800 shadow-sm transition-colors duration-300 fade-in-up" ref="headerRef">
      <div class="max-w-4xl mx-auto px-6 py-8">
        <div class="flex flex-col md:flex-row items-center md:items-start gap-6">
          <div class="flex-shrink-0">
            <div class="w-32 h-32 bg-gradient-to-br from-blue-500 to-purple-600 rounded-full flex items-center justify-center text-white text-4xl font-bold shadow-lg">
              {{ initials }}
            </div>
          </div>
          <div class="text-center md:text-left flex-grow">
            <h1 class="text-4xl font-bold text-gray-900 dark:text-white mb-2 transition-colors duration-300">{{ personalInfo.name }}</h1>
            <h2 class="text-xl text-blue-600 dark:text-blue-400 mb-4 transition-colors duration-300">{{ personalInfo.title }}</h2>
            <p class="text-gray-600 dark:text-gray-300 mb-4 max-w-2xl transition-colors duration-300">{{ personalInfo.summary }}</p>
            <div class="flex flex-wrap justify-center md:justify-start gap-4 text-sm text-gray-600 dark:text-gray-400 transition-colors duration-300">
              <div class="flex items-center gap-1">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
                  <path d="M2.003 5.884L10 9.882l7.997-3.998A2 2 0 0016 4H4a2 2 0 00-1.997 1.884z"></path>
                  <path d="M18 8.118l-8 4-8-4V14a2 2 0 002 2h12a2 2 0 002-2V8.118z"></path>
                </svg>
                {{ personalInfo.email }}
              </div>
              <div class="flex items-center gap-1">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd" d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd"></path>
                </svg>
                {{ personalInfo.location }}
              </div>            
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Menu lateral minimizável -->
    <div v-if="sidebarMinimized" class="fixed left-0 top-1/2 -translate-y-1/2 z-40">
      <button @click="toggleSidebar" class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-full shadow p-2 w-10 h-10 flex items-center justify-center transition-colors duration-300" :aria-label="'Abrir menu lateral'" :title="'Abrir menu lateral'">
        <svg v-if="!isDark" class="w-7 h-7 text-gray-700" fill="currentColor" viewBox="0 0 20 20"><path d="M7 5l5 5-5 5V5z"/></svg>
        <svg v-else class="w-7 h-7 text-gray-200" fill="currentColor" viewBox="0 0 20 20"><path d="M7 5l5 5-5 5V5z"/></svg>
      </button>
    </div>
    <nav v-else class="fixed left-0 top-1/2 -translate-y-1/2 z-40 bg-white dark:bg-gray-800 rounded-lg shadow-lg p-2 flex flex-col gap-2 border border-gray-200 dark:border-gray-700 min-w-[140px]">
      <button @click="toggleSidebar" class="absolute -right-4 top-1/2 -translate-y-1/2 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-full shadow p-2 w-10 h-10 flex items-center justify-center transition-colors duration-300" :aria-label="'Minimizar menu lateral'" :title="'Minimizar menu lateral'">
        <svg v-if="!isDark" class="w-7 h-7 text-gray-700" fill="currentColor" viewBox="0 0 20 20"><path d="M13 15l-5-5 5-5v10z"/></svg>
        <svg v-else class="w-7 h-7 text-gray-200" fill="currentColor" viewBox="0 0 20 20"><path d="M13 15l-5-5 5-5v10z"/></svg>
      </button>
      <a href="#top" class="flex items-center gap-2 text-xs text-gray-700 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 transition-colors duration-300 mt-6" title="Topo" aria-label="Ir para o topo">
        <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M10 2l6 6H4l6-6z"/></svg>Início
      </a>
      <a href="#experienceRef" class="flex items-center gap-2 text-xs text-gray-700 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 transition-colors duration-300" title="Experiência" aria-label="Ir para experiência">
        <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M6 6V5a3 3 0 013-3h2a3 3 0 013 3v1h2a2 2 0 012 2v3.57A22.952 22.952 0 0110 13a22.95 22.95 0 01-8-1.43V8a2 2 0 012-2h2z"/></svg>Experiência.
      </a>
      <a href="#projectsRef" class="flex items-center gap-2 text-xs text-gray-700 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 transition-colors duration-300" title="Projetos" aria-label="Ir para projetos">
        <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M3 4a1 1 0 011-1h12a1 1 0 011 1v2a1 1 0 01-1 1H4a1 1 0 01-1-1V4z"/></svg>Participações.
      </a>
      <a href="#skillsRef" class="flex items-center gap-2 text-xs text-gray-700 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 transition-colors duration-300" title="Skills" aria-label="Ir para skills">
        <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>Skills
      </a>
      <a href="#educationRef" class="flex items-center gap-2 text-xs text-gray-700 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 transition-colors duration-300" title="Formação" aria-label="Ir para formação">
        <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M10.394 2.08a1 1 0 00-.788 0l-7 3a1 1 0 000 1.84L5.25 8.051a.999.999 0 01.356-.257l4-1.714a1 1 0 11.788 1.838L7.667 9.088l1.94.831a1 1 0 00.787 0l7-3a1 1 0 000-1.838l-7-3z"/></svg>Formação.
      </a>
      <a href="#contactRef" class="flex items-center gap-2 text-xs text-gray-700 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 transition-colors duration-300 mb-2" title="Contato" aria-label="Ir para contato">
        <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M13 6a3 3 0 11-6 0 3 3 0 016 0zM18 8a2 2 0 11-4 0 2 2 0 014 0zM14 15a4 4 0 00-8 0v3h8v-3z"/></svg>Contato
      </a>
    </nav>

    <!-- Estado do menu lateral minimizável -->
    <div class="fixed right-4 top-4 z-40">
      <button 
        @click="toggleSidebar" 
        class="p-3 bg-white dark:bg-gray-800 rounded-full shadow-lg hover:shadow-xl transition-all duration-300 border border-gray-200 dark:border-gray-700"
        :class="{ flip: flipActive }"
        aria-label="Alternar modo escuro"
        title="Alternar modo escuro"
      >
        <svg v-if="!isDark" class="w-5 h-5 text-gray-600" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z" clip-rule="evenodd"></path>
        </svg>
        <svg v-else class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
          <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"></path>
        </svg>
      </button>
    </div>

    <!-- Main Content -->
    <main class="max-w-4xl mx-auto px-6 py-8">
      <div class="space-y-8">
        <!-- Experience Section -->
            <!-- Experience Section -->
            <section id="experienceRef" class="bg-white dark:bg-gray-800 rounded-lg shadow-sm p-6 transition-colors duration-300 fade-in-up" ref="experienceRef">
            <h3 class="text-2xl font-bold mb-6 flex items-center gap-2 bg-gradient-to-r from-blue-500 to-purple-600 text-transparent bg-clip-text">
              <svg class="w-6 h-6 text-blue-600 dark:text-blue-400" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M6 6V5a3 3 0 013-3h2a3 3 0 013 3v1h2a2 2 0 012 2v3.57A22.952 22.952 0 0110 13a22.95 22.95 0 01-8-1.43V8a2 2 0 012-2h2zm2-1a1 1 0 011-1h2a1 1 0 011 1v1H8V5zm1 5a1 1 0 011-1h.01a1 1 0 110 2H10a1 1 0 01-1-1z" clip-rule="evenodd"></path>
                <path d="M2 13.692V16a2 2 0 002 2h12a2 2 0 002-2v-2.308A24.974 24.974 0 0110 15c-2.796 0-5.487-.46-8-1.308z"></path>
              </svg>
              Experiência profissional
            </h3>
            <div class="space-y-6">
              <div v-for="job in experience" :key="job.id" class="border-l-4 border-blue-500 dark:border-blue-400 pl-4">
                <div class="flex flex-col sm:flex-row sm:justify-between sm:items-start mb-2">
                  <h4 class="text-lg font-semibold text-gray-900 dark:text-white transition-colors duration-300">{{ job.position }}</h4>
                  <span class="text-sm text-gray-500 dark:text-gray-400 transition-colors duration-300">{{ job.duration }}</span>
                </div>
                <p class="text-blue-600 dark:text-blue-400 font-medium mb-2 transition-colors duration-300">{{ job.company }}</p>
                <p class="text-gray-600 dark:text-gray-300 mb-3 transition-colors duration-300">{{ job.description }}</p>
                <ul class="list-disc list-inside text-gray-600 dark:text-gray-300 space-y-1 transition-colors duration-300">
                  <li v-for="achievement in job.achievements" :key="achievement">{{ achievement }}</li>
                </ul>
              </div>
            </div>
          </section>

          <!-- Projects Section -->
            <section id="projectsRef" class="bg-white dark:bg-gray-800 rounded-lg shadow-sm p-6 transition-colors duration-300 fade-in-up" ref="projectsRef">
            <h3 class="text-2xl font-bold mb-6 flex items-center gap-2 bg-gradient-to-r from-blue-500 to-purple-600 text-transparent bg-clip-text">
              <svg class="w-6 h-6 text-blue-600 dark:text-blue-400" fill="currentColor" viewBox="0 0 20 20">
                <path d="M3 4a1 1 0 011-1h12a1 1 0 011 1v2a1 1 0 01-1 1H4a1 1 0 01-1-1V4zM3 10a1 1 0 011-1h6a1 1 0 011 1v6a1 1 0 01-1 1H4a1 1 0 01-1-1v-6zM14 9a1 1 0 00-1 1v6a1 1 0 001 1h2a1 1 0 001-1v-6a1 1 0 00-1-1h-2z"></path>
              </svg>
              Participações
            </h3>
            <div class="grid gap-6">
              <div v-for="project in projects"  :key="project.id" class="border border-gray-200 dark:border-gray-600 rounded-lg p-4 hover:shadow-md dark:hover:shadow-xl transition-all duration-300">
                <div class="flex justify-between items-start mb-2">
                  <h4 class="text-lg font-semibold text-gray-900 dark:text-white transition-colors duration-300">{{ project.name }}</h4>
                  <div class="flex gap-2">
                    <a v-if="project.demo"  :href="project.demo" class="text-gray-400 hover:text-gray-600 dark:text-gray-500 dark:hover:text-gray-300 transition-colors duration-300"
                       :aria-label="'Link para ' + project.name"
                       :title="project.name">
                      <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                        <path d="M11 3a1 1 0 00-.788 0l-7 3a1 1 0 000 1.84L5.25 8.051a.999.999 0 01.356-.257l4-1.714a1 1 0 11.788 1.838L7.667 9.088l1.94.831a1 1 0 00.787 0l7-3a1 1 0 000-1.838l-7-3zM3.31 9.397L5 10.12v4.102a8.969 8.969 0 00-1.05-.174 1 1 0 01-.89-.89 11.115 11.115 0 01.25-3.762zM9.3 16.573A9.026 9.026 0 007 14.935v-3.957l1.818.78a3 3 0 002.364 0l5.508-2.361a11.026 11.026 0 01.25 3.762 1 1 0 01-.89.89 8.968 8.968 0 00-5.35 2.524 1 1 0 01-1.4 0zM6 18a1 1 0 001-1v-2.065a8.935 8.935 0 00-2-.712V17a1 1 0 001 1z"></path>
                      </svg>
                    </a>
                  </div>
                </div>
                <p class="text-gray-600 dark:text-gray-300 mb-3 transition-colors duration-300">{{ project.description }}</p>
                <div class="flex flex-wrap gap-2">
                  <span v-for="tech in project.technologies" :key="tech" class="px-2 py-1 bg-blue-100 dark:bg-blue-900 text-blue-800 dark:text-blue-200 text-xs rounded-full transition-colors duration-300 cursor-pointer hover:bg-blue-200 dark:hover:bg-blue-800" :title="tech" :aria-label="'Tecnologia: ' + tech">
                    {{ tech }}
                  </span>
                </div>
              </div>
            </div>
          </section>

        <!-- Skills Section -->
          <section id="skillsRef" class="bg-white dark:bg-gray-800 rounded-lg shadow-sm p-6 transition-colors duration-300 fade-in-up" ref="skillsRef">
            <h3 class="text-xl font-bold mb-4 flex items-center gap-2 bg-gradient-to-r from-blue-500 to-purple-600 text-transparent bg-clip-text">
              <svg class="w-5 h-5 text-blue-600 dark:text-blue-400" fill="currentColor" viewBox="0 0 20 20">
                <path d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
              </svg>
              Skills
            </h3>
            <div class="space-y-4">
              <div v-for="skillCategory in skills" :key="skillCategory.category">
                <h4 class="font-semibold text-gray-700 dark:text-gray-300 mb-2 transition-colors duration-300">{{ skillCategory.category }}</h4>
                <div class="flex flex-wrap gap-2">
                  <span v-for="skill in skillCategory.items" :key="skill" class="px-2 py-1 bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-200 text-sm rounded transition-colors duration-300 cursor-pointer hover:bg-blue-200 dark:hover:bg-blue-800" :title="skill" :aria-label="'Skill: ' + skill">
                    {{ skill }}
                  </span>
                </div>
              </div>
            </div>
          </section>

          <!-- Education Section -->
            <section id="educationRef" class="bg-white dark:bg-gray-800 rounded-lg shadow-sm p-6 transition-colors duration-300 fade-in-up" ref="educationRef">
            <h3 class="text-xl font-bold mb-4 flex items-center gap-2 bg-gradient-to-r from-blue-500 to-purple-600 text-transparent bg-clip-text">
              <svg class="w-5 h-5 text-blue-600 dark:text-blue-400" fill="currentColor" viewBox="0 0 20 20">
                <path d="M10.394 2.08a1 1 0 00-.788 0l-7 3a1 1 0 000 1.84L5.25 8.051a.999.999 0 01.356-.257l4-1.714a1 1 0 11.788 1.838L7.667 9.088l1.94.831a1 1 0 00.787 0l7-3a1 1 0 000-1.838l-7-3zM3.31 9.397L5 10.12v4.102a8.969 8.969 0 00-1.05-.174 1 1 0 01-.89-.89 11.115 11.115 0 01.25-3.762zM9.3 16.573A9.026 9.026 0 007 14.935v-3.957l1.818.78a3 3 0 002.364 0l5.508-2.361a11.026 11.026 0 01.25 3.762 1 1 0 01-.89.89 8.968 8.968 0 00-5.35 2.524 1 1 0 01-1.4 0zM6 18a1 1 0 001-1v-2.065a8.935 8.935 0 00-2-.712V17a1 1 0 001 1z"></path>
              </svg>
              Formação
            </h3>
            <div class="space-y-4">
              <div v-for="edu in education" :key="edu.id">
                <h4 class="font-semibold text-gray-900 dark:text-white transition-colors duration-300">{{ edu.degree }}</h4>
                <p class="text-blue-600 dark:text-blue-400 transition-colors duration-300">{{ edu.institution }}</p>
                <p class="text-sm text-gray-500 dark:text-gray-400 transition-colors duration-300">{{ edu.year }}</p>
                <p v-if="edu.details" class="text-gray-600 dark:text-gray-300 text-sm mt-1 transition-colors duration-300">{{ edu.details }}</p>
              </div>
            </div>
          </section>

          <!-- Contact/Links Section -->
            <section id="contactRef" class="bg-white dark:bg-gray-800 rounded-lg shadow-sm p-6 transition-colors duration-300 fade-in-up" ref="contactRef">
            <h3 class="text-xl font-bold mb-4 flex items-center gap-2 bg-gradient-to-r from-blue-500 to-purple-600 text-transparent bg-clip-text">
              <svg class="w-5 h-5 text-blue-600 dark:text-blue-400" fill="currentColor" viewBox="0 0 20 20">
                <path d="M13 6a3 3 0 11-6 0 3 3 0 016 0zM18 8a2 2 0 11-4 0 2 2 0 014 0zM14 15a4 4 0 00-8 0v3h8v-3z"></path>
              </svg>
              Contato
            </h3>
            <div class="space-y-3">
              <a v-for="link in socialLinks" 
                 :target="link.name === 'Portfolio' ? '_self' : '_blank'" 
                 :key="link.name" 
                 :href="link.url" 
                 class="flex items-center gap-3 text-gray-600 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 transition-colors duration-300"
                 :aria-label="'Link para ' + link.name"
                 :title="link.name">
                <div v-html="link.icon" class="w-5 h-5" :title="link.name" :aria-label="link.name"></div>
                <span>{{ link.name }}</span>
              </a>
            </div>
          </section>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, onMounted, onUnmounted } from 'vue'

// Estado do menu lateral minimizável
const sidebarMinimized = ref(true)
const toggleSidebar = () => {
  sidebarMinimized.value = !sidebarMinimized.value
}
const flipActive = ref(false)
const isDark = ref(false)

// Refs for animated elements
const headerRef = ref<HTMLElement | null>(null)
const experienceRef = ref<HTMLElement | null>(null)
const projectsRef = ref<HTMLElement | null>(null)
const skillsRef = ref<HTMLElement | null>(null)
const educationRef = ref<HTMLElement | null>(null)
const contactRef = ref<HTMLElement | null>(null)

// Intersection Observer for scroll animations
let observer: IntersectionObserver | null = null
let lastScrollY = 0
let isScrollingDown = true

// Track scroll direction
const handleScroll = () => {
  const currentScrollY = window.scrollY
  isScrollingDown = currentScrollY > lastScrollY
  lastScrollY = currentScrollY
}

const setupScrollAnimations = () => {
  const options = {
    root: null,
    rootMargin: '0px 0px -50px 0px', // Trigger when element is 50px from bottom of viewport
    threshold: 0.1
  }

  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting && isScrollingDown) {
        // Animate when scrolling down and entering viewport
        entry.target.classList.add('animate-fade-in-up')
        entry.target.classList.remove('fade-in-up')
      } else if (!entry.isIntersecting && !isScrollingDown) {
        // Reset when scrolling up and leaving viewport
        entry.target.classList.remove('animate-fade-in-up')
        entry.target.classList.add('fade-in-up')
      }
    })
  }, options)

  // Observe all sections
  const sections = [
    headerRef.value,
    experienceRef.value,
    projectsRef.value,
    skillsRef.value,
    educationRef.value,
    contactRef.value
  ]

  sections.forEach((section) => {
    if (section) {
      // Initially hide the sections
      section.classList.add('fade-in-up')
      observer?.observe(section)
    }
  })
}

const toggleDarkMode = () => {
  flipActive.value = true
  isDark.value = !isDark.value
  localStorage.setItem('darkMode', JSON.stringify(isDark.value))
  updateDarkMode()
  setTimeout(() => {
    flipActive.value = false
  }, 600) // tempo igual ao da animação flip
}

const updateDarkMode = () => {
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
}

onMounted(() => {
  const savedDarkMode = localStorage.getItem('darkMode')
  if (savedDarkMode) {
    isDark.value = JSON.parse(savedDarkMode)
  } else {
    isDark.value = window.matchMedia('(prefers-color-scheme: dark)').matches
  }
  updateDarkMode()
  
  // Add scroll listener for direction tracking
  window.addEventListener('scroll', handleScroll, { passive: true })
  
  // Setup scroll animations after component is mounted
  setTimeout(setupScrollAnimations, 100)
})

onUnmounted(() => {
  if (observer) {
    observer.disconnect()
  }
  window.removeEventListener('scroll', handleScroll)
})

const personalInfo = {
  name: "Samuel Araujo",
  title: "Full Stack Developer",
  summary: "Desenvolvedor Full Stack (PHP/Laravel + Vue.js) atuando em sistemas críticos do Porto de Santos. Experiência em sustentação, resolução de incidentes em produção, refatoração de APIs e melhoria de performance e usabilidade. Foco em entregas end-to-end com escalabilidade, eficiência operacional e experiência do usuário.",
  email: "samuellarujo13@gmail.com",
  location: "Santos, São Paulo"
}

const initials = computed(() => {
  return personalInfo.name
    .split(' ')
    .map(word => word.charAt(0))
    .join('')
    .toUpperCase()
})

const experience = [
  {
    id: 1,
    position: "Desenvolvedor Full Stack (PHP/Laravel) — Pleno",
    company: "MSB",
    duration: "abr/2025 - Presente",
    description: "Atuação na sustentação e evolução de sistemas críticos que suportam operações logísticas do Porto de Santos, com foco em desempenho, modernização e melhoria contínua das plataformas.",
    achievements: [      
      "Atuação direta na resolução de incidentes em produção, garantindo alta disponibilidade dos sistemas",
      "Refatoração de aplicações legadas, aumentando estabilidade e reduzindo recorrência de falhas",
      "Desenvolvimento e manutenção de APIs REST em Laravel com foco em performance e escalabilidade",
      "Criação de interfaces modernas e responsivas com Vue.js, TypeScript e TailwindCSS",
      "Integração full stack entre serviços e aplicações, melhorando a experiência do usuário e eficiência operacional",  
      "Atuação em projetos de inovação e prototipação de soluções",
      "Atuação em ambiente ágil, contribuindo para entregas contínuas e alinhadas ao negócio"
    ]
  },
  {
    id: 2,
    position: "Desenvolvedor Full Stack (Sustentação) — Sistemas Internos",
    company: "Paipe",
    duration: "Set/2024 - Abr/2025",
    description: "Atuação em sistemas críticos da Autoridade Portuária de Santos, com foco em sustentação, resolução de incidentes e melhoria contínua de aplicações.",
    achievements: [
      "Redução de 70% do backlog de chamados, elevando a eficiência operacional da equipe",
      "Atuação direta na resolução de incidentes em produção, incluindo falhas de acesso e regras de negócio",
      "Debug e correção de aplicações em PHP/Laravel e Oracle SQL, aumentando estabilidade e confiabilidade",
      "Evolução de sistemas legados com melhorias estruturais e funcionais",
      "Implementação de regras de controle de acesso, fortalecendo a segurança e governança dos sistemas",
      "Parceria com áreas de negócio para melhoria de UX e aderência às necessidades operacionais",
      "Participação em cerimônias ágeis (Scrum), contribuindo para entregas contínuas e priorização eficiente"
    ]
  },
  {
    id: 3,
    position: "Estagiário em Desenvolvimento de Software",
    company: "Autoridade Portuária de Santos (APS)",
    duration: "Mai/2023 - Jul/2024",
    description: "Atuação em sistemas corporativos e operacionais do Porto de Santos, contribuindo para a manutenção, evolução e confiabilidade de aplicações utilizadas em processos críticos.",
    achievements: [
      "Atuação na correção de incidentes e bugs em produção, garantindo continuidade dos sistemas internos",
      "Manutenção e evolução do site institucional, melhorando a experiência do usuário e acesso à informação",
      "Suporte ao sistema de atracação portuária, com consultas e ajustes em banco de dados para assegurar integridade e confiabilidade operacional",
      "Desenvolvimento e manutenção de funcionalidades em PHP e banco de dados",
      "Participação em rotinas ágeis, contribuindo com entregas alinhadas às demandas do negócio",
      "Vivência em ambiente corporativo de grande porte, com foco em qualidade, estabilidade e boas práticas de desenvolvimento"
    ]
  }
]

const projects = [
  {
    id: 1,
    name: "Portal do cliente e fornecedor",
    description: "Plataforma digital que centraliza serviços e comunicações entre a Autoridade Portuária de Santos, clientes e fornecedores, permitindo solicitação e acompanhamento de serviços, gestão de credenciamentos e integração de processos administrativos e financeiros em um único ambiente online. Atuação em manutenção, correção de bugs em produção, melhorias de performance e evolução de funcionalidades em Laravel.",    technologies: ["PHP", "Laravel", "MySQL"],    
    demo: "https://portaldocliente.portodesantos.com.br/login"
  },
  {
    id: 2,
    name: "Site do porto de santos",
    description: "Site oficial da Autoridade Portuária de Santos, voltado à divulgação de informações institucionais, operacionais e de transparência. Reúne dados sobre infraestrutura portuária, estatísticas de movimentação, notícias, serviços ao público e acesso a sistemas corporativos do porto.",
    technologies: ["PHP", "Laravel", "MySQL"],
    demo: "https://www.portodesantos.com.br/"
  },
  {
    id: 3,
    name: "Nova intranet do porto de Santos",
    description: "Rebranding e modernização da intranet do Porto de Santos, com foco em melhoria de UX, performance e organização da informação, mantendo compatibilidade com funcionalidades legadas. O projeto foi entregue em produção, proporcionando uma experiência mais intuitiva e eficiente para os usuários internos.",
    technologies: ["Vue.js", "Laravel","MySQL","TypeScript", "Tailwind CSS"],
    demo: "https://intra.portodesantos.com.br/"
 },
  // {
  //   id: 4,
  //   name: "Novo portal do cliente e fornecedor - Em desenvolvimento...",
  //   description: "O projeto tem como objetivo reconstruir o portal do cliente e fornecedor da Autoridade Portuária de Santos, preservando suas funcionalidades e características consolidadas. A nova versão proporcionará uma experiência mais moderna, intuitiva e alinhada às necessidades atuais dos usuários.",
  //   technologies: ["Vue.js", "Laravel","MySQL","TypeScript", "Tailwind CSS"],
  //  // demo: "#"
  // }
]

const skills = [
  {
    category: "Frontend",
    items: ["Vue.js", "TypeScript", "JavaScript", "HTML5", "CSS3", "Tailwind CSS"]
  },
  {
    category: "Backend",
    items: ["PHP", "Laravel", "MySQL", "SQL"]
  },
  {
    category: "Idiomas",
    items: ["Inglês fluente", "Português nativo"]
  },
  {
    category: "Tools",
    items: ["Git", "Docker", "Vite", "Webpack", "Insomnia", "Dbeaver", "Scrum"]

  },
  {
    category: "Soft Skills",
    items: ["Comunicação eficaz", "Trabalho em equipe", "Resolução de problemas", "Adaptabilidade", "Proatividade", "Foco em resultados"]
  }
]

const education = [
  {
    id: 1,
    degree: "Analise e Desenvolvimento de Sistemas",
    institution: "Fatec - Rubens Lara",
    year: "2021 - 2025",
    details: "Graduado em Análise e Desenvolvimento de Sistemas"
  },
  {
    id: 2,
    degree: "Fundamentos do PHP",
    institution: "Rocketseat",
    year: "2025",
    details: "Treinamento prático voltado à revisão da base do PHP, estrutura de código e manipulação de dados."
  },
  {
    id: 3,
    degree: "IA para DEVs - Como o desenvolvedor extrai o máximo com IA",
    institution: "Udemy",
    year: "2025",
    details: "Treinamento focado na aplicação de IA no desenvolvimento de software, incluindo fundamentos de modelos de linguagem (LLMs), engenharia de prompts e uso de IA para geração de código, troubleshooting e aumento de produtividade no dia a dia do desenvolvimento."  }

]

const socialLinks = [
  {
    name: "LinkedIn",
    url: "https://www.linkedin.com/in/samuel-araujo-3210261b6",
    icon: `<svg fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.338 16.338H13.67V12.16c0-.995-.017-2.277-1.387-2.277-1.39 0-1.601 1.086-1.601 2.207v4.248H8.014v-8.59h2.559v1.174h.037c.356-.675 1.227-1.387 2.526-1.387 2.703 0 3.203 1.778 3.203 4.092v4.711zM5.005 6.575a1.548 1.548 0 11-.003-3.096 1.548 1.548 0 01.003 3.096zm-1.337 9.763H6.34v-8.59H3.667v8.59zM17.668 1H2.328C1.595 1 1 1.581 1 2.298v15.403C1 18.418 1.595 19 2.328 19h15.34c.734 0 1.332-.582 1.332-1.299V2.298C19 1.581 18.402 1 17.668 1z" clip-rule="evenodd"></path></svg>`
  },
  {
    name: "GitHub",
    url: "https://github.com/AraujoSam",
    icon: `<svg fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M10 0C4.477 0 0 4.484 0 10.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0110 4.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.203 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.942.359.31.678.921.678 1.856 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0020 10.017C20 4.484 15.522 0 10 0z" clip-rule="evenodd"></path></svg>`
  },
  {
    name: "Portfolio",
    url: "#top",
    icon: `<svg fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M12.586 4.586a2 2 0 112.828 2.828l-3 3a2 2 0 01-2.828 0 1 1 0 00-1.414 1.414 4 4 0 005.656 0l3-3a4 4 0 00-5.656-5.656l-1.5 1.5a1 1 0 101.414 1.414l1.5-1.5zm-5 5a2 2 0 012.828 0 1 1 0 101.414-1.414 4 4 0 00-5.656 0l-3 3a4 4 0 105.656 5.656l1.5-1.5a1 1 0 10-1.414-1.414l-1.5 1.5a2 2 0 11-2.828-2.828l3-3z" clip-rule="evenodd"></path></svg>`
  },
  {
    name: "Email",
    url: `mailto:${personalInfo.email}`,
    icon: `<svg fill="currentColor" viewBox="0 0 20 20"><path d="M2.003 5.884L10 9.882l7.997-3.998A2 2 0 0016 4H4a2 2 0 00-1.997 1.884z"></path><path d="M18 8.118l-8 4-8-4V14a2 2 0 002 2h12a2 2 0 002-2V8.118z"></path></svg>`
  }
]
</script>

<style scoped>
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.flip {
  animation: flipAnim 0.6s;
}

@keyframes flipAnim {
  0% { transform: rotateY(0deg); }
  50% { transform: rotateY(180deg); }
  100% { transform: rotateY(360deg); }
}

/* Initial hidden state */
.fade-in-up {
  opacity: 0;
  transform: translateY(30px);
}

/* Animation class */
.animate-fade-in-up {
  animation: fadeInUp 0.7s ease-out forwards;
}

/* Suave transição de cores para dark mode */
html, body {
  scroll-behavior: smooth;
  transition: background-color 0.5s, color 0.5s;
}
</style>