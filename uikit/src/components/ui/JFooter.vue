<script setup lang="ts">
type FooterVariant = 'default' | 'minimal' | 'bordered' | 'dark'

const props = withDefaults(defineProps<{
  variant?: FooterVariant
}>(), {
  variant: 'default'
})

const variants: Record<FooterVariant, string> = {
  default: 'bg-white border-t border-slate-200',
  minimal: 'bg-transparent border-t border-slate-100',
  bordered: 'bg-white border-t-2 border-slate-900',
  dark: 'bg-slate-900 text-white'
}
</script>

<template>
  <footer :class="['mt-auto transition-all', variants[variant]]">
    <slot />
  </footer>
</template>
