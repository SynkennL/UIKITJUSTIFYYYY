<script setup lang="ts">
import { Navbar, Footer } from '@/components'

interface Props {
    title?: string
    subtitle?: string
    showNavbar?: boolean
    showFooter?: boolean
}

withDefaults(defineProps<Props>(), {
    title: 'Justify UI',
    subtitle: 'UI Kütüphanesi',
    showNavbar: true,
    showFooter: true
})
</script>

<template>
    <div class="min-h-screen flex flex-col">
        <Navbar v-if="showNavbar" :title="title" />

        <main class="flex-1 py-10 px-6">
            <div class="max-w-4xl mx-auto space-y-16">
                <!-- Hero Section -->
                <section class="text-center py-8">
                    <h1 class="text-3xl font-bold text-slate-900 mb-2">{{ title }}</h1>
                    <p class="text-slate-500 mb-4">{{ subtitle }}</p>
                    <div class="flex justify-center gap-2 flex-wrap">
                        <span class="px-3 py-1 bg-slate-100 text-slate-600 text-xs rounded-full">UI Kit</span>
                    </div>
                </section>

                <div class="h-px bg-gradient-to-r from-transparent via-slate-200 to-transparent" />

                <!-- Content Slot -->
                <slot />
            </div>
        </main>

        <Footer v-if="showFooter" />
    </div>
</template>