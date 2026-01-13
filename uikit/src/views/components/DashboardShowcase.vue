<script setup lang="ts">
import { ref } from 'vue'
import { JDashboard } from '@/components'

const menuItems = [
    { label: 'Dashboard' },
    { label: 'Projeler' },
    { label: 'Kullanıcılar' },
    { label: 'Raporlar' },
    { label: 'Ayarlar' }
]

const selectedContent = ref('Dashboard')

const handleSelect = (item: { label: string }) => {
    selectedContent.value = item.label
}

// Örnek stat verileri
const stats = [
    { label: 'Toplam Kullanıcı', value: '2,847', change: '+12%' },
    { label: 'Aktif Oturum', value: '1,234', change: '+8%' },
    { label: 'Gelir', value: '₺48.5K', change: '-2%' },
    { label: 'Dönüşüm', value: '3.42%', change: '+1%' }
]

// Örnek grafik verileri
const chartData = [35, 60, 45, 80, 55, 70, 90]
</script>

<template>
    <section class="space-y-6">
        <div class="flex items-center gap-3">
            <h2 class="text-xl font-semibold text-white">Dashboard</h2>
            <span class="text-xs text-slate-400">Admin Panel Komponenti</span>
        </div>

        <!-- Admin Panel Örneği -->
        <div class="p-6 bg-slate-100 rounded-xl space-y-4">
            <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Admin Panel Dashboard</p>
            <div class="rounded-xl border border-slate-300 overflow-hidden h-80 bg-white">
                <JDashboard :items="menuItems" title="Admin Panel" sidebar-width="sm" default-active="Dashboard"
                    @select="handleSelect">
                    <div class="space-y-4">
                        <!-- Stat Cards -->
                        <div class="grid grid-cols-4 gap-3">
                            <div v-for="stat in stats" :key="stat.label"
                                class="p-3 bg-white rounded-lg border border-slate-200">
                                <p class="text-xs text-slate-500">{{ stat.label }}</p>
                                <p class="text-lg font-bold text-slate-900">{{ stat.value }}</p>
                                <p
                                    :class="['text-xs', stat.change.startsWith('+') ? 'text-green-600' : 'text-red-500']">
                                    {{ stat.change }}</p>
                            </div>
                        </div>

                        <!-- Simple Chart -->
                        <div class="p-3 bg-white rounded-lg border border-slate-200">
                            <p class="text-xs text-slate-500 mb-2">Haftalık Özet</p>
                            <div class="flex items-end gap-2 h-16">
                                <div v-for="(val, i) in chartData" :key="i" class="flex-1 bg-slate-200 rounded-t"
                                    :style="{ height: val + '%' }"></div>
                            </div>
                        </div>
                    </div>
                </JDashboard>
            </div>
        </div>

        <!-- Kompakt Dashboard -->
        <div class="p-6 bg-slate-100 rounded-xl space-y-4">
            <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Kompakt Dashboard</p>
            <div class="rounded-xl border border-slate-300 overflow-hidden h-56 bg-white">
                <JDashboard :items="menuItems.slice(0, 3)" title="Panel" sidebar-width="sm" default-active="Dashboard">
                    <div class="grid grid-cols-2 gap-3">
                        <div class="p-3 bg-white rounded-lg border border-slate-200">
                            <p class="text-xs text-slate-500">Kullanıcı</p>
                            <p class="text-xl font-bold text-slate-900">248</p>
                        </div>
                        <div class="p-3 bg-white rounded-lg border border-slate-200">
                            <p class="text-xs text-slate-500">Proje</p>
                            <p class="text-xl font-bold text-slate-900">12</p>
                        </div>
                    </div>
                </JDashboard>
            </div>
        </div>

        <!-- Geniş Sidebar -->
        <div class="p-6 bg-slate-100 rounded-xl space-y-4">
            <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Geniş Sidebar</p>
            <div class="rounded-xl border border-slate-300 overflow-hidden h-56 bg-white">
                <JDashboard :items="menuItems" title="Yönetim Paneli" sidebar-width="lg" default-active="Dashboard">
                    <div class="p-4 bg-white rounded-lg border border-slate-200">
                        <p class="text-sm text-slate-600">Geniş sidebar ile daha fazla alan</p>
                    </div>
                </JDashboard>
            </div>
        </div>
    </section>
</template>
