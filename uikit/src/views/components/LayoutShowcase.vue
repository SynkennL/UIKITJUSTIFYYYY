<script setup lang="ts">
import { JNavbar, JFooter, JButton } from '@/components'

// Örnek nav items
const navItems = [
  { label: 'Ana Sayfa', href: '#', active: true },
  { label: 'Hakkında', href: '#' },
  { label: 'İletişim', href: '#' }
]
</script>

<template>
  <section class="space-y-6">
    <div class="flex items-center gap-3">
      <h2 class="text-xl font-semibold text-slate-900">Layout</h2>
      <span class="text-xs text-slate-400">Navbar & Footer</span>
    </div>

    <!-- Navbar -->
    <div class="space-y-4">
      <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Navbar</p>

      <!-- Default Navbar with Mobile Menu -->
      <div class="rounded-xl overflow-hidden border border-slate-200">
        <JNavbar variant="default" :sticky="false" title="Default" :nav-items="navItems">
          <template #actions>
            <JButton variant="solid" class="px-3 py-1.5 text-sm rounded-lg">Giriş</JButton>
          </template>
          <template #mobile-actions>
            <JButton variant="solid" class="w-full justify-center">Giriş Yap</JButton>
          </template>
        </JNavbar>
      </div>

      <!-- Glass Navbar -->
      <div class="rounded-xl overflow-hidden border border-slate-200 bg-gradient-to-r from-slate-100 to-slate-50">
        <JNavbar variant="glass" :sticky="false" title="Glass" :nav-items="[{ label: 'Link', href: '#' }]" />
      </div>

      <!-- Bordered Navbar -->
      <div class="rounded-xl overflow-hidden border border-slate-200">
        <JNavbar variant="bordered" :sticky="false" title="Bordered" :nav-items="[{ label: 'Link', href: '#' }]" />
      </div>

      <!-- Custom Content Navbar -->
      <div class="rounded-xl overflow-hidden border border-slate-200">
        <JNavbar variant="default" :sticky="false">
          <div class="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between">
            <span class="font-semibold text-slate-900">Custom Content</span>
            <nav class="flex items-center gap-4">
              <a href="#" class="text-sm text-slate-500 hover:text-slate-900">Ana Sayfa</a>
              <a href="#" class="text-sm text-slate-500 hover:text-slate-900">Hakkında</a>
            </nav>
            <JButton variant="solid" class="px-3 py-1.5 text-sm rounded-lg">Giriş</JButton>
          </div>
        </JNavbar>
      </div>
    </div>

    <!-- Footer -->
    <div class="space-y-4">
      <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Footer</p>

      <div class="rounded-xl overflow-hidden border border-slate-200">
        <JFooter variant="default">
          <div class="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between text-sm text-slate-500">
            <span>Default Footer</span>
            <span>&copy; 2025</span>
          </div>
        </JFooter>
      </div>

      <div class="rounded-xl overflow-hidden border border-slate-200">
        <JFooter variant="dark">
          <div class="max-w-6xl mx-auto px-6 py-6 text-center">
            <p class="font-medium">Dark Footer</p>
            <p class="text-sm text-slate-400 mt-1">&copy; 2025</p>
          </div>
        </JFooter>
      </div>
    </div>
  </section>
</template>
