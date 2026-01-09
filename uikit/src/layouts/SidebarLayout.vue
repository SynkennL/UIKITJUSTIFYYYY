<script setup lang="ts">
import { ref, computed } from 'vue'
import { Navbar, Footer } from '@/components'

interface Props {
    title?: string
    sidebarWidth?: 'sm' | 'md' | 'lg'
    sidebarPosition?: 'left' | 'right'
    collapsible?: boolean
    showNavbar?: boolean
    showFooter?: boolean
}

const props = withDefaults(defineProps<Props>(), {
    title: 'Justify UI',
    sidebarWidth: 'md',
    sidebarPosition: 'left',
    collapsible: true,
    showNavbar: true,
    showFooter: true
})

const isCollapsed = ref(false)
const isMobileOpen = ref(false)

const widthClass = computed(() => {
    if (isCollapsed.value) return 'w-16'
    return { sm: 'w-52', md: 'w-64', lg: 'w-80' }[props.sidebarWidth]
})

const toggle = () => isCollapsed.value = !isCollapsed.value
const toggleMobile = () => isMobileOpen.value = !isMobileOpen.value
const closeMobile = () => isMobileOpen.value = false
</script>

<template>
    <div class="min-h-screen flex flex-col bg-slate-900">
        <!-- Navbar -->
        <Navbar v-if="showNavbar" :title="title">
            <template #left>
                <button class="lg:hidden p-2 rounded-lg text-slate-400 hover:bg-slate-700" @click="toggleMobile">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M4 6h16M4 12h16M4 18h16" />
                    </svg>
                </button>
            </template>
        </Navbar>

        <div class="flex flex-1">
            <!-- Mobile Overlay -->
            <div v-if="isMobileOpen" class="fixed inset-0 bg-black/50 z-40 lg:hidden" @click="closeMobile" />

            <!-- Sidebar -->
            <aside :class="[
                'fixed lg:static inset-y-0 z-50 bg-slate-800 border-r border-slate-700 flex flex-col transition-all duration-300',
                sidebarPosition === 'left' ? 'left-0' : 'right-0',
                widthClass,
                isMobileOpen ? 'translate-x-0' : (sidebarPosition === 'left' ? '-translate-x-full lg:translate-x-0' : 'translate-x-full lg:translate-x-0')
            ]" :style="{ top: showNavbar ? '64px' : '0' }">
                <!-- Sidebar Header -->
                <div class="flex items-center justify-between p-4 border-b border-slate-700">
                    <span v-if="!isCollapsed" class="font-semibold text-slate-100">Menu</span>
                    <button v-if="collapsible" @click="toggle"
                        class="p-1.5 rounded-lg text-slate-400 hover:bg-slate-700 hidden lg:block">
                        <svg :class="['w-4 h-4', isCollapsed && 'rotate-180']" fill="none" stroke="currentColor"
                            viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M11 19l-7-7 7-7m8 14l-7-7 7-7" />
                        </svg>
                    </button>
                    <button class="p-1.5 rounded-lg text-slate-400 hover:bg-slate-700 lg:hidden" @click="closeMobile">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>

                <!-- Sidebar Content Slot -->
                <nav class="flex-1 overflow-y-auto p-3">
                    <slot name="sidebar" :collapsed="isCollapsed" />
                </nav>
            </aside>

            <!-- Main Content -->
            <main :class="['flex-1 p-6', sidebarPosition === 'right' && 'order-first']">
                <slot />
            </main>
        </div>

        <Footer v-if="showFooter" />
    </div>
</template>
