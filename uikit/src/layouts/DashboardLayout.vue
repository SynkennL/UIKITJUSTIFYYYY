<script setup lang="ts">
import { ref, computed } from 'vue'

interface Props {
    title?: string
    sidebarWidth?: 'sm' | 'md' | 'lg'
}

const props = withDefaults(defineProps<Props>(), {
    title: 'Dashboard',
    sidebarWidth: 'md'
})

const isMobileOpen = ref(false)

const sidebarWidthClass = computed(() => {
    return { sm: 'w-52', md: 'w-60', lg: 'w-72' }[props.sidebarWidth]
})

const closeMobile = () => isMobileOpen.value = false
const openMobile = () => isMobileOpen.value = true
</script>

<template>
    <div class="min-h-screen flex flex-col bg-slate-900">
        <!-- Top Header -->
        <header
            class="flex items-center justify-between h-14 px-4 lg:px-6 bg-slate-800 border-b border-slate-700 shrink-0">
            <div class="flex items-center gap-4">
                <!-- Mobile Menu -->
                <button @click="openMobile" class="p-2 -ml-2 rounded-lg text-slate-400 hover:bg-slate-700 lg:hidden">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M4 6h16M4 12h16M4 18h16" />
                    </svg>
                </button>
                <span class="font-semibold text-slate-100">{{ title }}</span>
            </div>
            <div class="flex items-center gap-2">
                <slot name="header-actions">
                    <button class="p-2 rounded-lg text-slate-400 hover:bg-slate-700">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                        </svg>
                    </button>
                    <button class="p-2 rounded-lg text-slate-400 hover:bg-slate-700">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6 6 0 10-12 0v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
                        </svg>
                    </button>
                    <div class="w-8 h-8 rounded-full bg-slate-600 ml-1"></div>
                </slot>
            </div>
        </header>

        <div class="flex flex-1 overflow-hidden">
            <!-- Mobile Overlay -->
            <div v-if="isMobileOpen" class="fixed inset-0 bg-black/50 z-40 lg:hidden" @click="closeMobile" />

            <!-- Sidebar -->
            <aside :class="[
                'fixed lg:static inset-y-0 left-0 z-50 bg-slate-800 border-r border-slate-700 flex flex-col transition-transform duration-300 lg:translate-x-0',
                sidebarWidthClass,
                isMobileOpen ? 'translate-x-0' : '-translate-x-full'
            ]" style="top: 56px;">
                <!-- Mobile Close -->
                <div class="flex items-center justify-between p-4 border-b border-slate-700 lg:hidden">
                    <span class="font-semibold text-slate-100">Menu</span>
                    <button @click="closeMobile" class="p-1.5 rounded-lg text-slate-400 hover:bg-slate-700">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>

                <!-- Sidebar Content -->
                <nav class="flex-1 overflow-y-auto p-3">
                    <slot name="sidebar" />
                </nav>

                <!-- Sidebar Footer -->
                <div v-if="$slots['sidebar-footer']" class="p-3 border-t border-slate-700">
                    <slot name="sidebar-footer" />
                </div>
            </aside>

            <!-- Main Content -->
            <main class="flex-1 p-4 lg:p-6 overflow-auto">
                <slot />
            </main>
        </div>
    </div>
</template>
