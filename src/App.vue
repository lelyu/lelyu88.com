<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import WorldGlobe from './components/WorldGlobe.vue'

const isDark = ref(false)

onMounted(() => {
  const saved = localStorage.getItem('theme')
  if (saved) {
    isDark.value = saved === 'dark'
  } else {
    isDark.value = window.matchMedia('(prefers-color-scheme: dark)').matches
  }
})

watch(isDark, (value) => {
  document.documentElement.classList.toggle('dark', value)
  localStorage.setItem('theme', value ? 'dark' : 'light')
})

function toggleDark() {
  isDark.value = !isDark.value
}
</script>

<template>
  <div
    class="min-h-screen bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 font-sans selection:bg-gray-200 dark:selection:bg-gray-700"
  >
    <button
      @click="toggleDark"
      class="fixed top-4 left-4 z-50 p-2 rounded-lg text-gray-500 dark:text-gray-400 hover:text-gray-900 dark:hover:text-gray-100 hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors"
      aria-label="Toggle dark mode"
    >
      <svg
        v-if="isDark"
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5"
        viewBox="0 0 20 20"
        fill="currentColor"
      >
        <path
          fill-rule="evenodd"
          d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"
          clip-rule="evenodd"
        />
      </svg>
      <svg
        v-else
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5"
        viewBox="0 0 20 20"
        fill="currentColor"
      >
        <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" />
      </svg>
    </button>

    <!-- Mobile: Globe centered in middle of context -->
    <div class="lg:hidden w-full h-[400px] flex items-center justify-center pt-16">
      <WorldGlobe :is-dark="isDark" />
    </div>

    <!-- Desktop: Two-column layout -->
    <div class="flex flex-col lg:flex-row">
      <!-- Left column: Globe (fixed on desktop) -->
      <aside
        class="hidden lg:flex lg:w-1/2 lg:h-screen lg:fixed lg:left-0 lg:top-0 items-center justify-center"
      >
        <WorldGlobe :is-dark="isDark" />
      </aside>

      <!-- Right column: Bio content -->
      <main class="w-full lg:w-1/2 lg:ml-[50%]">
        <div class="max-w-2xl mx-auto px-6 py-20 md:py-32">
          <header class="mb-12">
            <h1 class="text-4xl font-bold tracking-tight mb-2">Le Lyu</h1>
            <p class="text-xl text-gray-500 dark:text-gray-400 font-light">
              Software Engineer & Creative Technologist
            </p>
          </header>

          <div class="space-y-8 text-lg leading-relaxed text-gray-700 dark:text-gray-300">
            <p>
              Hi, I go by <strong>Lok</strong>. I am a software engineer currently based in Hong
              Kong, working at the
              <a href="https://cpii.hk/" target="_blank" class="link"
                >Centre for Perceptual and Interactive Intelligence (CPII)</a
              >.
            </p>

            <p>
              My journey has been a bit of a global tour. I grew up in <strong>Guangzhou</strong>,
              spent five formative years in <strong>Massachusetts</strong> for university and early
              work, and have now landed in the vibrant tech scene of Hong Kong.
            </p>

            <p>
              I specialize in building the <strong>frontend for agentic AI applications</strong>.
              While my expertise lies in crafting high-fidelity, responsive interfaces using
              <strong>Vue.js</strong> and <strong>TypeScript</strong>, I am deeply interested in the
              entire AI stack. Currently, I'm bridging the gap between design and logic by diving
              into <strong>LangGraph</strong>, learning how to better orchestrate the backend agents
              that power my UIs.
            </p>

            <p>
              I obsess over performance and type safety. Recently, I implemented Strategy Design
              Patterns to refactor legacy modules, achieving a
              <span class="font-medium text-gray-900 dark:text-gray-100"
                >10x increase in processing speed</span
              >. I also enjoy the challenge of real-time interactions—wiring up WebSockets to stream
              LLM responses seamlessly to the client.
            </p>

            <p>
              I am a Class of 2024 graduate from
              <a href="https://www.bc.edu/" target="_blank" class="link">Boston College</a>, where I
              double-majored in <strong>Computer Science</strong> and <strong>Economics</strong>.
              During my time in the US, I also enjoyed teaching advanced Python concepts to students
              at <em>iCode</em>.
            </p>

            <div class="pt-4">
              <p
                class="text-base text-gray-500 dark:text-gray-400 uppercase tracking-wider font-semibold mb-3"
              >
                Core Technologies
              </p>
              <ul class="flex flex-wrap gap-x-6 gap-y-2 text-base text-gray-600 dark:text-gray-400">
                <li>Vue.js / TypeScript</li>
                <li>Python (FastAPI)</li>
                <li>Tailwind CSS</li>
                <li>LangGraph (Learning)</li>
              </ul>
            </div>
          </div>

          <footer class="mt-20 pt-8 border-t border-gray-100 dark:border-gray-800">
            <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
              <div class="flex gap-6">
                <a href="https://www.linkedin.com/in/lelyu/" target="_blank" class="link-subtle">
                  LinkedIn
                </a>
                <a href="mailto:lyulelok@gmail.com" class="link-subtle"> Email </a>
                <a href="https://github.com/" target="_blank" class="link-subtle"> GitHub </a>
              </div>

              <p class="text-sm text-gray-400">Hong Kong</p>
            </div>

            <div
              class="mt-10 p-4 bg-gray-50 dark:bg-gray-800 rounded-lg text-sm text-gray-600 dark:text-gray-400 italic"
            >
              <span class="font-semibold not-italic block mb-1 dark:text-gray-200">P.S.</span>
              If you are also in Hong Kong and enjoy outdoor activities (e.g., hiking on weekends),
              feel free to reach out via LinkedIn.
            </div>
          </footer>
        </div>
      </main>
    </div>
  </div>
</template>

<style scoped>
.link {
  color: #111827;
  border-bottom: 1px solid #d1d5db;
  transition:
    color 200ms,
    border-color 200ms;
  padding-bottom: 0.125rem;
}

.link:hover {
  color: #2563eb;
  border-bottom-color: #3b82f6;
}

.dark .link {
  color: #f3f4f6;
  border-bottom-color: #6b7280;
}

.dark .link:hover {
  color: #60a5fa;
  border-bottom-color: #3b82f6;
}

.link-subtle {
  color: #6b7280;
  transition: color 200ms;
}

.link-subtle:hover {
  color: #111827;
}

.dark .link-subtle {
  color: #9ca3af;
}

.dark .link-subtle:hover {
  color: #f3f4f6;
}

.dark main strong {
  color: #f3f4f6;
}
</style>
