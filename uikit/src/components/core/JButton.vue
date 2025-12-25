<script setup lang="ts">
import type { ButtonVariant } from '@/types/components'

const props = withDefaults(defineProps<{
  variant?: ButtonVariant
}>(), {
  variant: 'solid'
})

const variants: Record<ButtonVariant, string> = {
  solid: 'bg-slate-800 text-white hover:bg-slate-700',
  outline: 'border border-slate-300 text-slate-700 hover:bg-slate-50',
  ghost: 'text-slate-600 hover:bg-slate-100',
  link: 'text-slate-600 hover:underline'
}
</script>

<template>
  <button :class="variants[props.variant]" class="transition-all cursor-pointer">
    <slot />
  </button>
</template>
