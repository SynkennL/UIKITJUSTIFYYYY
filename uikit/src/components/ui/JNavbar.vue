<script setup lang="ts">
type NavbarVariant = 'default' | 'transparent' | 'bordered' | 'glass'

const props = withDefaults(defineProps<{
  variant?: NavbarVariant
  sticky?: boolean
}>(), {
  variant: 'default',
  sticky: true
})

const variants: Record<NavbarVariant, string> = {
  default: 'bg-white border-b border-slate-200',
  transparent: 'bg-transparent',
  bordered: 'bg-white border-b-2 border-slate-900',
  glass: 'bg-white/80 backdrop-blur-md border-b border-slate-200/50'
}
</script>

<template>
  <header :class="[
    'w-full z-50 transition-all',
    sticky ? 'sticky top-0' : '',
    variants[variant]
  ]">
    <slot />
  </header>
</template>
