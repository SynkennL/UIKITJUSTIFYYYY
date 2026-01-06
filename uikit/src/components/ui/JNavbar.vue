<script setup lang="ts">
import { ref, computed } from 'vue'

type NavbarVariant = 'default' | 'transparent' | 'bordered' | 'glass'

interface NavItem {
  label: string
  href?: string
  onClick?: () => void
  active?: boolean
}

interface Props {
  variant?: NavbarVariant
  sticky?: boolean
  title?: string
  showMobileMenu?: boolean
  navItems?: NavItem[]
  containerWidth?: 'sm' | 'md' | 'lg' | 'xl' | '2xl' | 'full'
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'default',
  sticky: true,
  title: '',
  showMobileMenu: true,
  navItems: () => [],
  containerWidth: 'lg'
})

// Mobil menü state
const isMobileMenuOpen = ref(false)

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

// Variant stilleri
const variants: Record<NavbarVariant, string> = {
  default: 'bg-white border-b border-slate-200',
  transparent: 'bg-transparent',
  bordered: 'bg-white border-b-2 border-slate-900',
  glass: 'bg-white/80 backdrop-blur-md border-b border-slate-200/50'
}

// Container genişlik sınıfları
const containerWidths: Record<string, string> = {
  'sm': 'max-w-xl',
  'md': 'max-w-2xl',
  'lg': 'max-w-4xl',
  'xl': 'max-w-6xl',
  '2xl': 'max-w-7xl',
  'full': 'w-full'
}

const containerClass = computed(() => containerWidths[props.containerWidth])
</script>

<template>
  <header :class="[
    'w-full z-50 transition-all',
    sticky ? 'sticky top-0' : '',
    variants[variant]
  ]">
    <!-- Default Slot veya varsayılan yapı -->
    <slot>
      <div :class="['mx-auto px-6', containerClass]">
        <div class="flex h-14 items-center justify-between">
          <!-- Sol Taraf: Logo & Nav -->
          <div class="flex items-center gap-6">
            <!-- Logo Slot veya Title -->
            <slot name="logo">
              <span v-if="title" class="font-bold text-slate-900">{{ title }}</span>
            </slot>

            <!-- Desktop Navigation -->
            <nav v-if="navItems.length" class="hidden md:flex items-center gap-4 text-sm">
              <slot name="nav-items">
                <a v-for="item in navItems" :key="item.label" :href="item.href || '#'" @click="item.onClick" :class="[
                  'transition-colors',
                  item.active
                    ? 'text-slate-900 font-medium'
                    : 'text-slate-500 hover:text-slate-900'
                ]">
                  {{ item.label }}
                </a>
              </slot>
            </nav>
          </div>

          <!-- Sağ Taraf -->
          <div class="flex items-center gap-3">
            <!-- Actions Slot (Desktop) -->
            <div class="hidden md:flex items-center gap-3">
              <slot name="actions" />
            </div>

            <!-- Mobile Menu Button -->
            <button v-if="showMobileMenu && navItems.length" @click="toggleMobileMenu"
              class="md:hidden p-2 text-slate-500 hover:text-slate-900 hover:bg-slate-100 rounded-lg transition-all"
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
        <div v-if="showMobileMenu && isMobileMenuOpen" class="md:hidden border-t border-slate-200 bg-white">
          <nav :class="['flex flex-col px-6 py-4 space-y-1', containerClass, 'mx-auto']">
            <!-- Mobile Nav Items -->
            <slot name="mobile-nav-items">
              <a v-for="item in navItems" :key="item.label" :href="item.href || '#'"
                @click="() => { item.onClick?.(); closeMobileMenu(); }" :class="[
                  'px-3 py-2 rounded-lg transition-colors',
                  item.active
                    ? 'text-slate-900 bg-slate-50 font-medium'
                    : 'text-slate-600 hover:text-slate-900 hover:bg-slate-50'
                ]">
                {{ item.label }}
              </a>
            </slot>

            <!-- Mobile Actions Slot -->
            <div class="pt-2 border-t border-slate-100 mt-2">
              <slot name="mobile-actions" />
            </div>
          </nav>
        </div>
      </Transition>
    </slot>
  </header>
</template>
