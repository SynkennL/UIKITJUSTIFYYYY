<script setup lang="ts">
const props = withDefaults(defineProps<{
  placeholder?: string
  disabled?: boolean
}>(), {
  placeholder: '',
  disabled: false
})

const model = defineModel<string>()
</script>

<template>
  <textarea v-model="model" :placeholder="placeholder" :disabled="disabled" :class="[
    'bg-slate-800 border border-slate-600 text-slate-100 placeholder-slate-500 transition-all focus:outline-none focus:border-slate-400 focus:ring-2 focus:ring-slate-500/30 resize-none',
    disabled ? 'opacity-50 cursor-not-allowed' : ''
  ]" />
</template>
