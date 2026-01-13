<script setup lang="ts">
import { ref, computed } from 'vue'

interface MenuItem {
    label: string
    children?: MenuItem[]
}

interface Props {
    items?: MenuItem[]
    title?: string
    sidebarWidth?: 'sm' | 'md' | 'lg'
    defaultActive?: string
}

const props = withDefaults(defineProps<Props>(), {
    items: () => [],
    title: 'Dashboard',
    sidebarWidth: 'md',
    defaultActive: ''
})

const emit = defineEmits<{
    select: [item: MenuItem]
}>()

const expandedItems = ref<string[]>([])
const activeItem = ref<string>(props.defaultActive)

const sidebarWidthClass = computed(() => {
    return { sm: 'w-44', md: 'w-52', lg: 'w-60' }[props.sidebarWidth]
})

const toggleExpand = (label: string) => {
    const index = expandedItems.value.indexOf(label)
    if (index > -1) {
        expandedItems.value.splice(index, 1)
    } else {
        expandedItems.value.push(label)
    }
}

const selectItem = (item: MenuItem) => {
    activeItem.value = item.label
    emit('select', item)
}

defineExpose({ activeItem, expandedItems })
</script>

<template>
    <div class="flex flex-col h-full overflow-hidden bg-white">
        <!-- Top Header -->
        <header class="flex items-center justify-between h-11 px-4 border-b border-slate-200 shrink-0 bg-slate-50">
            <div class="flex items-center gap-3">
                <span class="font-semibold text-slate-800 text-sm">{{ title }}</span>
            </div>
            <div class="flex items-center gap-2">
                <slot name="header-actions">
                    <!-- Search -->
                    <button class="p-1.5 rounded text-slate-400 hover:text-slate-600 hover:bg-slate-200">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                        </svg>
                    </button>
                    <!-- Notification -->
                    <button class="p-1.5 rounded text-slate-400 hover:text-slate-600 hover:bg-slate-200">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6 6 0 10-12 0v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
                        </svg>
                    </button>
                    <!-- User -->
                    <div class="w-6 h-6 rounded-full bg-slate-300"></div>
                </slot>
            </div>
        </header>

        <div class="flex flex-1 overflow-hidden">
            <!-- Sidebar -->
            <aside :class="['shrink-0 flex flex-col border-r border-slate-200 bg-white', sidebarWidthClass]">
                <nav class="flex-1 overflow-y-auto p-2 space-y-1">
                    <template v-for="item in items" :key="item.label">
                        <!-- Simple Item -->
                        <button v-if="!item.children" type="button" :class="[
                            'w-full flex items-center px-3 py-1.5 rounded-lg text-sm transition-colors',
                            activeItem === item.label
                                ? 'bg-slate-900 text-white'
                                : 'text-slate-600 hover:bg-slate-100'
                        ]" @click="selectItem(item)">
                            {{ item.label }}
                        </button>

                        <!-- Expandable -->
                        <div v-else>
                            <button type="button" :class="[
                                'w-full flex items-center justify-between px-3 py-1.5 rounded-lg text-sm transition-colors',
                                expandedItems.includes(item.label) ? 'bg-slate-100' : 'text-slate-600 hover:bg-slate-100'
                            ]" @click="toggleExpand(item.label)">
                                <span>{{ item.label }}</span>
                                <svg class="w-3 h-3 transition-transform"
                                    :class="{ 'rotate-180': expandedItems.includes(item.label) }" fill="none"
                                    stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M19 9l-7 7-7-7" />
                                </svg>
                            </button>
                            <div v-if="expandedItems.includes(item.label)" class="ml-3 mt-1 space-y-1">
                                <button v-for="child in item.children" :key="child.label" type="button" :class="[
                                    'w-full text-left px-3 py-1 rounded text-sm',
                                    activeItem === child.label ? 'bg-slate-900 text-white' : 'text-slate-500 hover:bg-slate-100'
                                ]" @click="selectItem(child)">
                                    {{ child.label }}
                                </button>
                            </div>
                        </div>
                    </template>
                    <slot name="sidebar" />
                </nav>
            </aside>

            <!-- Content -->
            <main class="flex-1 p-4 overflow-auto bg-slate-50">
                <slot :active="activeItem" />
            </main>
        </div>
    </div>
</template>
