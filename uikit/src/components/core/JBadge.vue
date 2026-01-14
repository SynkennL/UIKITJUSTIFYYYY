<script setup lang="ts">
import type { BadgeVariant } from '@/types/components'

const props = withDefaults(defineProps<{
  variant?: BadgeVariant
}>(), {
  variant: 'solid'
})

const variants: Record<BadgeVariant, string> = {
  solid: 'bg-slate-600 text-white',
  outline: 'bg-transparent border border-slate-500 text-slate-300',
  soft: 'bg-slate-700 text-slate-200',
  dot: 'bg-transparent text-slate-300 before:content-[""] before:w-1.5 before:h-1.5 before:rounded-full before:bg-current before:mr-1.5',
  gradient: 'bg-gradient-to-r from-slate-600 to-slate-500 text-white',
  glass: 'bg-slate-700/40 backdrop-blur-sm text-slate-200 border border-slate-600/30'
}
</script>

<template>
  <span :class="variants[variant]"
    class="inline-flex items-center px-3 py-1.5 rounded-full text-sm font-semibold transition-all">
    <slot />
  </span>
</template>
