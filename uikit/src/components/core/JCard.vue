<script setup lang="ts">
import type { CardVariant } from '@/types/components'

const props = withDefaults(defineProps<{
  variant?: CardVariant
}>(), {
  variant: 'default'
})

const variants: Record<CardVariant, string> = {
  default: 'bg-slate-800 border border-slate-700',
  bordered: 'bg-slate-800 border-2 border-slate-500',
  elevated: 'bg-slate-800 shadow-xl shadow-slate-900/50',
  glass: 'bg-slate-800/60 backdrop-blur-xl border border-slate-600/30 shadow-lg'
}
</script>

<template>
  <div :class="variants[variant]" class="rounded-xl overflow-hidden transition-all">
    <slot />
  </div>
</template>
