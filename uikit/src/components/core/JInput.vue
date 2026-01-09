<script setup lang="ts">
import type { InputVariant } from '@/types/components'

const props = withDefaults(defineProps<{
  variant?: InputVariant
  placeholder?: string
  type?: string
  disabled?: boolean
}>(), {
  variant: 'default',
  placeholder: '',
  type: 'text',
  disabled: false
})

const model = defineModel<string>()

const variants: Record<InputVariant, string> = {
  default: 'bg-slate-800 border border-slate-600 focus:border-slate-400 focus:ring-2 focus:ring-slate-500/30',
  filled: 'bg-slate-700 border border-transparent focus:bg-slate-800 focus:border-slate-500',
  underline: 'bg-transparent border-b-2 border-slate-600 rounded-none focus:border-slate-400 px-0',
  ghost: 'bg-transparent border border-transparent hover:bg-slate-700 focus:bg-slate-700'
}
</script>

<template>
  <input v-model="model" :type="type" :placeholder="placeholder" :disabled="disabled" :class="[
    variants[variant],
    'text-slate-100 placeholder-slate-500 transition-all focus:outline-none',
    disabled ? 'opacity-50 cursor-not-allowed' : ''
  ]" />
</template>
