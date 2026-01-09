<script setup lang="ts">
import type { ButtonVariant } from '@/types/components'

const props = withDefaults(defineProps<{
  variant?: ButtonVariant
}>(), {
  variant: 'solid'
})

const variants: Record<ButtonVariant, string> = {
  solid: 'bg-slate-600 text-white hover:bg-slate-500',
  outline: 'border border-slate-500 text-slate-300 hover:bg-slate-700',
  ghost: 'text-slate-300 hover:bg-slate-700',
  link: 'text-slate-400 hover:text-slate-200 hover:underline'
}
</script>

<template>
  <button :class="variants[props.variant]" class="transition-all cursor-pointer">
    <slot />
  </button>
</template>
