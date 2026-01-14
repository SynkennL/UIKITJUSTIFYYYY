<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'

interface NavLink {
  name: string
  to?: string
  href?: string
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

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

// Navigasyon linkleri - Home sayfası veya Components sayfasına göre değişir
const navLinks = computed<NavLink[]>(() => {
  if (props.variant === 'home') {
    return [
      { name: 'Home', to: '/' },
      { name: 'Components', to: '/components' }
    ]
  }
  return [
    { name: 'Home', to: '/' },
    { name: 'Components', to: '/components' },
    { name: 'Buttons', href: '#buttons' },
    { name: 'Inputs', href: '#inputs' },
    { name: 'Cards', href: '#cards' }
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
              <a v-else :href="link.href" class="text-slate-400 hover:text-slate-100 transition-colors">
                {{ link.name }}
              </a>
            </template>
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
            <a v-else :href="link.href" @click="closeMobileMenu"
              class="px-3 py-2 text-slate-300 hover:text-slate-100 hover:bg-slate-800 rounded-lg transition-colors">
              {{ link.name }}
            </a>
          </template>

          <!-- Mobile GitHub Link -->
          <a href="https://github.com/SynkennL/JustifyyUIKit" target="_blank" rel="noopener noreferrer"
            @click="closeMobileMenu"
            class="flex items-center gap-2 px-3 py-2 text-slate-300 hover:text-slate-100 hover:bg-slate-800 rounded-lg transition-colors">
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
