<script setup lang="ts">
import { computed } from 'vue'
import { Navbar, Footer } from '@/components'

interface Props {
    title?: string
    subtitle?: string
    showNavbar?: boolean
    showFooter?: boolean
    showHero?: boolean
    containerWidth?: 'sm' | 'md' | 'lg' | 'xl' | 'full'
}

const props = withDefaults(defineProps<Props>(), {
    title: 'Justify UI',
    subtitle: 'Modern Vue.js UI Component Library',
    showNavbar: true,
    showFooter: true,
    showHero: true,
    containerWidth: 'xl'
})

const containerClass = computed(() => {
    const widths: Record<NonNullable<Props['containerWidth']>, string> = {
        sm: 'max-w-2xl',
        md: 'max-w-3xl',
        lg: 'max-w-4xl',
        xl: 'max-w-6xl',
        full: 'max-w-full'
    }
    return widths[props.containerWidth]
})
</script>

<template>
    <div class="min-h-screen flex flex-col">
        <Navbar v-if="showNavbar" :title="title" />

        <main class="flex-1 py-10 px-6">
            <div :class="[containerClass, 'mx-auto space-y-16']">
                <!-- Hero Section (Optional) -->
                <section v-if="showHero" class="text-center py-8">
                    <slot name="hero">
                        <h1 class="text-3xl font-bold text-slate-100 mb-2">{{ title }}</h1>
                        <p class="text-slate-400 mb-4">{{ subtitle }}</p>
                        <div class="flex justify-center gap-2 flex-wrap">
                            <span class="px-3 py-1 bg-slate-700 text-slate-300 text-xs rounded-full">UI Kit</span>
                        </div>
                    </slot>
                </section>

                <div v-if="showHero" class="h-px bg-gradient-to-r from-transparent via-slate-600 to-transparent" />

                <!-- Content Slot -->
                <slot />
            </div>
        </main>

        <Footer v-if="showFooter" />
    </div>
</template>