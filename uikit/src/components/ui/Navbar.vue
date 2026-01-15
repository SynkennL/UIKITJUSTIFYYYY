<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { useRoute } from 'vue-router'

interface NavLink {
  name: string
  to?: string
  href?: string
}

interface ComponentCategory {
  name: string
  href: string
  icon: string
  description: string
}

interface Props {
  title?: string
  variant?: 'default' | 'home'
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Justify UI',
  variant: 'default'
})

const route = useRoute()

// Mobil menü durumu
const isMobileMenuOpen = ref(false)
// Dropdown durumu
const isComponentsDropdownOpen = ref(false)
const dropdownRef = ref<HTMLElement | null>(null)

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

const toggleComponentsDropdown = () => {
  isComponentsDropdownOpen.value = !isComponentsDropdownOpen.value
}

const closeComponentsDropdown = () => {
  isComponentsDropdownOpen.value = false
}

// Component kategorileri - dropdown içinde gösterilecek
const componentCategories: ComponentCategory[] = [
  {
    name: 'Buttons',
    href: '#buttons',
    icon: 'M15.75 5.25a3 3 0 013 3m3 0a6 6 0 01-7.029 5.912c-.563-.097-1.159.026-1.563.43L10.5 17.25H8.25v2.25H6v2.25H2.25v-2.818c0-.597.237-1.17.659-1.591l6.499-6.499c.404-.404.527-1 .43-1.563A6 6 0 1121.75 8.25z',
    description: 'Buton çeşitleri'
  },
  {
    name: 'Inputs',
    href: '#inputs',
    icon: 'M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L10.582 16.07a4.5 4.5 0 01-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 011.13-1.897l8.932-8.931zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0115.75 21H5.25A2.25 2.25 0 013 18.75V8.25A2.25 2.25 0 015.25 6H10',
    description: 'Form elemanları'
  },
  {
    name: 'Cards',
    href: '#cards',
    icon: 'M2.25 7.125C2.25 6.504 2.754 6 3.375 6h6c.621 0 1.125.504 1.125 1.125v3.75c0 .621-.504 1.125-1.125 1.125h-6a1.125 1.125 0 01-1.125-1.125v-3.75zM14.25 8.625c0-.621.504-1.125 1.125-1.125h5.25c.621 0 1.125.504 1.125 1.125v8.25c0 .621-.504 1.125-1.125 1.125h-5.25a1.125 1.125 0 01-1.125-1.125v-8.25zM3.75 16.125c0-.621.504-1.125 1.125-1.125h5.25c.621 0 1.125.504 1.125 1.125v2.25c0 .621-.504 1.125-1.125 1.125h-5.25a1.125 1.125 0 01-1.125-1.125v-2.25z',
    description: 'Kart tasarımları'
  },
  {
    name: 'Extras',
    href: '#extras',
    icon: 'M9.813 15.904L9 18.75l-.813-2.846a4.5 4.5 0 00-3.09-3.09L2.25 12l2.846-.813a4.5 4.5 0 003.09-3.09L9 5.25l.813 2.846a4.5 4.5 0 003.09 3.09L15.75 12l-2.846.813a4.5 4.5 0 00-3.09 3.09zM18.259 8.715L18 9.75l-.259-1.035a3.375 3.375 0 00-2.455-2.456L14.25 6l1.036-.259a3.375 3.375 0 002.455-2.456L18 2.25l.259 1.035a3.375 3.375 0 002.456 2.456L21.75 6l-1.035.259a3.375 3.375 0 00-2.456 2.456zM16.894 20.567L16.5 21.75l-.394-1.183a2.25 2.25 0 00-1.423-1.423L13.5 18.75l1.183-.394a2.25 2.25 0 001.423-1.423l.394-1.183.394 1.183a2.25 2.25 0 001.423 1.423l1.183.394-1.183.394a2.25 2.25 0 00-1.423 1.423z',
    description: 'Badge, Alert ve diğerleri'
  },
  {
    name: 'Progress',
    href: '#progress',
    icon: 'M3 13.125C3 12.504 3.504 12 4.125 12h2.25c.621 0 1.125.504 1.125 1.125v6.75C7.5 20.496 6.996 21 6.375 21h-2.25A1.125 1.125 0 013 19.875v-6.75zM9.75 8.625c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125v11.25c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 01-1.125-1.125V8.625zM16.5 4.125c0-.621.504-1.125 1.125-1.125h2.25C20.496 3 21 3.504 21 4.125v15.75c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 01-1.125-1.125V4.125z',
    description: 'İlerleme göstergeleri'
  },
  {
    name: 'Misc',
    href: '#misc',
    icon: 'M6.75 12a.75.75 0 11-1.5 0 .75.75 0 011.5 0zM12.75 12a.75.75 0 11-1.5 0 .75.75 0 011.5 0zM18.75 12a.75.75 0 11-1.5 0 .75.75 0 011.5 0z',
    description: 'Diğer bileşenler'
  },
  {
    name: 'Icons',
    href: '#icons',
    icon: 'M21 7.5l-9-5.25L3 7.5m18 0l-9 5.25m9-5.25v9l-9 5.25M3 7.5l9 5.25M3 7.5v9l9 5.25m0-9v9',
    description: 'İkon koleksiyonu'
  }
]

// Dışarı tıklamada dropdown'ı kapat
const handleClickOutside = (event: MouseEvent) => {
  if (dropdownRef.value && !dropdownRef.value.contains(event.target as Node)) {
    closeComponentsDropdown()
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})

// Navigasyon linkleri - sadece Home ve Components
const navLinks = computed<NavLink[]>(() => {
  return [
    { name: 'Home', to: '/' },
    { name: 'Components', to: '/components' }
  ]
})

const isActiveLink = (link: NavLink) => {
  if (link.to) {
    return route.path === link.to
  }
  return false
}
</script>

<template>
  <header
    class="sticky top-0 z-50 w-full border-b border-slate-700 bg-slate-900/95 backdrop-blur supports-[backdrop-filter]:bg-slate-900/80">
    <div class="max-w-6xl mx-auto px-6">
      <div class="flex h-14 items-center justify-between">
        <!-- Logo -->
        <div class="flex items-center gap-6">
          <router-link to="/" class="font-bold text-slate-100 hover:text-white transition-colors">
            {{ title }}
          </router-link>

          <!-- Desktop Navigation -->
          <nav class="hidden md:flex items-center gap-4 text-sm">
            <template v-for="link in navLinks" :key="link.name">
              <router-link v-if="link.to" :to="link.to" :class="[
                'transition-colors',
                isActiveLink(link) ? 'text-slate-100' : 'text-slate-400 hover:text-slate-100'
              ]">
                {{ link.name }}
              </router-link>
            </template>

            <!-- Components Dropdown - Sadece Components sayfasında göster -->
            <div v-if="variant === 'default'" ref="dropdownRef" class="relative">
              <button @click.stop="toggleComponentsDropdown" :class="[
                'flex items-center gap-1.5 px-3 py-1.5 rounded-lg transition-all text-sm font-medium',
                isComponentsDropdownOpen
                  ? 'text-slate-100 bg-slate-700'
                  : 'text-slate-400 hover:text-slate-100 hover:bg-slate-800'
              ]">
                <span>Keşfet</span>
                <svg
                  :class="['w-3.5 h-3.5 transition-transform duration-200', isComponentsDropdownOpen ? 'rotate-180' : '']"
                  fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
                </svg>
              </button>

              <!-- Dropdown Menu -->
              <Transition enter-active-class="transition duration-200 ease-out"
                enter-from-class="opacity-0 scale-95 -translate-y-1"
                enter-to-class="opacity-100 scale-100 translate-y-0"
                leave-active-class="transition duration-150 ease-in"
                leave-from-class="opacity-100 scale-100 translate-y-0"
                leave-to-class="opacity-0 scale-95 -translate-y-1">
                <div v-if="isComponentsDropdownOpen"
                  class="absolute top-full left-0 mt-2 w-[420px] p-3 bg-slate-800/95 backdrop-blur-sm border border-slate-700 rounded-xl shadow-2xl shadow-black/30 z-50">
                  <div class="px-2 py-2 mb-2">
                    <span class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Bileşen
                      Kategorileri</span>
                  </div>
                  <div class="grid grid-cols-2 gap-1">
                    <a v-for="category in componentCategories" :key="category.name" :href="category.href"
                      @click="closeComponentsDropdown"
                      class="flex items-center gap-3 px-3 py-3 rounded-lg hover:bg-slate-700/60 transition-all group">
                      <div
                        class="flex-shrink-0 w-9 h-9 flex items-center justify-center bg-slate-700/50 rounded-lg group-hover:bg-slate-600/50 transition-colors">
                        <svg class="w-4.5 h-4.5 text-slate-400 group-hover:text-slate-200 transition-colors" fill="none"
                          stroke="currentColor" viewBox="0 0 24 24">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" :d="category.icon" />
                        </svg>
                      </div>
                      <div class="min-w-0">
                        <span
                          class="block text-sm font-medium text-slate-300 group-hover:text-slate-100 transition-colors">{{
                            category.name }}</span>
                        <span
                          class="block text-xs text-slate-500 group-hover:text-slate-400 transition-colors truncate">{{
                            category.description }}</span>
                      </div>
                    </a>
                  </div>
                </div>
              </Transition>
            </div>
          </nav>
        </div>

        <!-- Right Side -->
        <div class="flex items-center gap-3">
          <!-- GitHub Link (Desktop) -->
          <a href="https://github.com/SynkennL/JustifyyUIKit" target="_blank" rel="noopener noreferrer"
            class="hidden md:flex p-2 text-slate-400 hover:text-slate-100 hover:bg-slate-700 rounded-lg transition-all"
            title="GitHub">
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
              <path fill-rule="evenodd"
                d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z"
                clip-rule="evenodd" />
            </svg>
          </a>

          <!-- Mobile Menu Button -->
          <button @click="toggleMobileMenu"
            class="md:hidden p-2 text-slate-400 hover:text-slate-100 hover:bg-slate-700 rounded-lg transition-all"
            :aria-expanded="isMobileMenuOpen" aria-label="Menüyü aç/kapat">
            <!-- Hamburger Icon -->
            <svg v-if="!isMobileMenuOpen" class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            </svg>
            <!-- Close Icon -->
            <svg v-else class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- Mobile Menu Panel -->
    <Transition enter-active-class="transition duration-200 ease-out" enter-from-class="opacity-0 -translate-y-2"
      enter-to-class="opacity-100 translate-y-0" leave-active-class="transition duration-150 ease-in"
      leave-from-class="opacity-100 translate-y-0" leave-to-class="opacity-0 -translate-y-2">
      <div v-if="isMobileMenuOpen" class="md:hidden border-t border-slate-700 bg-slate-900">
        <nav class="flex flex-col px-6 py-4 space-y-1">
          <template v-for="link in navLinks" :key="link.name">
            <router-link v-if="link.to" :to="link.to" @click="closeMobileMenu" :class="[
              'px-3 py-2 rounded-lg transition-colors',
              isActiveLink(link) ? 'text-slate-100 bg-slate-800' : 'text-slate-300 hover:text-slate-100 hover:bg-slate-800'
            ]">
              {{ link.name }}
            </router-link>
          </template>

          <!-- Mobile Component Categories - Sadece Components sayfasında göster -->
          <div v-if="variant === 'default'" class="pt-2 mt-2 border-t border-slate-700">
            <span class="block px-3 py-2 text-xs font-semibold text-slate-500 uppercase tracking-wider">Bileşen
              Kategorileri</span>
            <a v-for="category in componentCategories" :key="category.name" :href="category.href"
              @click="closeMobileMenu"
              class="flex items-center gap-3 px-3 py-2.5 rounded-lg hover:bg-slate-800 transition-colors group">
              <div
                class="flex-shrink-0 w-7 h-7 flex items-center justify-center bg-slate-700/50 rounded-lg group-hover:bg-slate-600/50 transition-colors">
                <svg class="w-3.5 h-3.5 text-slate-400 group-hover:text-slate-200 transition-colors" fill="none"
                  stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" :d="category.icon" />
                </svg>
              </div>
              <span class="text-sm text-slate-300 group-hover:text-slate-100 transition-colors">{{ category.name
                }}</span>
            </a>
          </div>

          <!-- Mobile GitHub Link -->
          <a href="https://github.com/SynkennL/JustifyyUIKit" target="_blank" rel="noopener noreferrer"
            @click="closeMobileMenu"
            class="flex items-center gap-2 px-3 py-2 mt-2 pt-2 border-t border-slate-700 text-slate-300 hover:text-slate-100 hover:bg-slate-800 rounded-lg transition-colors">
            <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
              <path fill-rule="evenodd"
                d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z"
                clip-rule="evenodd" />
            </svg>
            GitHub
          </a>
        </nav>
      </div>
    </Transition>
  </header>
</template>
