<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { MainLayout } from '@/layouts'
import { JButton, JCard, JBadge } from '@/components'

const router = useRouter()

// Tilt Card Refs
const cardRef = ref<HTMLElement | null>(null)
const glowRef = ref<HTMLElement | null>(null)
const contentRef = ref<HTMLElement | null>(null)

const navigateToComponents = () => {
    router.push('/components')
}

const openGitHub = () => {
    window.open('https://github.com/SynkennL/JustifyyUIKit', '_blank')
}

// FAQ Data
const faqs = ref([
    {
        question: 'Justify UI Kit nedir?',
        answer: 'Justify UI Kit, Vue.js için tasarlanmış modern ve özelleştirilebilir bir bileşen kütüphanesidir. Projelerinizi hızla geliştirmenizi sağlar.',
        isOpen: false
    },
    {
        question: 'Nasıl kullanmaya başlarım?',
        answer: 'GitHub reposundan projeyi klonlayarak veya npm ile kurarak hemen kullanmaya başlayabilirsiniz. Dokümantasyon sayfasından detaylı bilgi alabilirsiniz.',
        isOpen: false
    },
    {
        question: 'Ücretli bir plan var mı?',
        answer: 'Hayır, Justify UI Kit tamamen ücretsiz ve açık kaynaklıdır. Tüm bileşenleri herhangi bir ücret ödemeden kullanabilirsiniz.',
        isOpen: false
    },
    {
        question: 'TypeScript desteği var mı?',
        answer: 'Evet, tüm bileşenler TypeScript ile yazılmıştır ve tam tip desteği sağlar.',
        isOpen: false
    }
])

const toggleFaq = (index: number) => {
    const faq = faqs.value[index]
    if (faq) {
        faq.isOpen = !faq.isOpen
    }
}

// Tilt Card Mouse Events
const handleMouseMove = (e: MouseEvent) => {
    if (!cardRef.value || !glowRef.value || !contentRef.value) return

    const rect = cardRef.value.getBoundingClientRect()
    const x = e.clientX - rect.left
    const y = e.clientY - rect.top

    const centerX = rect.width / 2
    const centerY = rect.height / 2

    const percentX = (x - centerX) / centerX
    const percentY = -((y - centerY) / centerY)

    cardRef.value.style.transform = `perspective(1000px) rotateY(${percentX * 10}deg) rotateX(${percentY * 10}deg)`
    contentRef.value.style.transform = `translateZ(50px)`
    glowRef.value.style.opacity = '1'
    glowRef.value.style.backgroundImage = `
        radial-gradient(
            circle at ${x}px ${y}px, 
            rgba(99, 102, 241, 0.3),
            transparent
        )
    `
}

const handleMouseLeave = () => {
    if (!cardRef.value || !glowRef.value || !contentRef.value) return

    cardRef.value.style.transform = 'perspective(1000px) rotateY(0deg) rotateX(0deg)'
    contentRef.value.style.transform = 'translateZ(0px)'
    glowRef.value.style.opacity = '0'
}
</script>

<template>
    <MainLayout title="Justify UI" :show-hero="false" :show-footer="false" navbar-variant="home" container-width="full">
        <div class="relative min-h-[calc(100vh-56px)] flex flex-col -m-6 bg-slate-900">

            <!-- Hero Content -->
            <div
                class="flex-1 grid grid-cols-1 lg:grid-cols-2 gap-8 lg:gap-16 items-center max-w-7xl mx-auto px-4 sm:px-8 py-12 lg:py-16 relative z-10">
                <!-- Left Side - Text Content -->
                <div class="flex flex-col gap-6 text-center lg:text-left">
                    <h1 class="text-4xl sm:text-5xl lg:text-6xl font-extrabold leading-tight">
                        <span class="text-slate-200">Justify UI Kit</span>
                        <br />
                        <span class="text-slate-500">Modern Components</span>
                    </h1>

                    <p class="text-lg text-slate-400 leading-relaxed max-w-lg mx-auto lg:mx-0">
                        Modern ve özelleştirilebilir Vue.js bileşenleri ile projelerinizi hızla hayata geçirin.
                    </p>

                    <div class="flex flex-wrap gap-4 justify-center lg:justify-start">
                        <div class="flex items-center gap-2 text-slate-400 text-sm">
                            <svg class="w-5 h-5 text-slate-500" viewBox="0 0 24 24" fill="none">
                                <path
                                    d="M9 12L11 14L15 10M21 12C21 16.9706 16.9706 21 12 21C7.02944 21 3 16.9706 3 12C3 7.02944 7.02944 3 12 3C16.9706 3 21 7.02944 21 12Z"
                                    stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                    stroke-linejoin="round" />
                            </svg>
                            <span>20+ Bileşen</span>
                        </div>
                        <div class="flex items-center gap-2 text-slate-400 text-sm">
                            <svg class="w-5 h-5 text-slate-500" viewBox="0 0 24 24" fill="none">
                                <path d="M13 10V3L4 14H11V21L20 10H13Z" stroke="currentColor" stroke-width="2"
                                    stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                            <span>Çok Hızlı</span>
                        </div>
                        <div class="flex items-center gap-2 text-slate-400 text-sm">
                            <svg class="w-5 h-5 text-slate-500" viewBox="0 0 24 24" fill="none">
                                <path d="M4 6H20M4 12H20M4 18H20" stroke="currentColor" stroke-width="2"
                                    stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                            <span>Çoklu Varyant</span>
                        </div>
                    </div>

                    <div class="flex flex-wrap gap-4 justify-center lg:justify-start mt-2">
                        <JButton variant="solid" @click="navigateToComponents"
                            class="!px-6 !py-3 !rounded-xl !font-semibold !bg-slate-600 hover:!bg-slate-500 transition-all duration-300 hover:-translate-y-0.5">
                            <span>Bileşenleri Keşfet</span>
                            <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none">
                                <path d="M14 5L21 12M21 12L14 19M21 12H3" stroke="currentColor" stroke-width="2"
                                    stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                        </JButton>
                        <JButton variant="outline" @click="openGitHub"
                            class="!px-5 !py-3 !rounded-xl !font-semibold !border-slate-600 hover:!bg-slate-700/50 transition-all duration-300">
                            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                                <path fill-rule="evenodd"
                                    d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z"
                                    clip-rule="evenodd" />
                            </svg>
                            <span>GitHub</span>
                        </JButton>
                    </div>
                </div>

                <!-- Right Side - Tilt Card -->
                <div class="flex justify-center items-center" style="perspective: 1000px;">
                    <div ref="cardRef"
                        class="w-[320px] sm:w-[340px] h-[400px] sm:h-[420px] bg-slate-700 rounded-2xl shadow-2xl relative cursor-pointer transition-all duration-300 ease-out border border-slate-600"
                        style="transform-style: preserve-3d;" @mousemove="handleMouseMove"
                        @mouseleave="handleMouseLeave">
                        <div ref="glowRef"
                            class="absolute inset-0 rounded-2xl opacity-0 transition-opacity duration-300 pointer-events-none">
                        </div>
                        <div ref="contentRef"
                            class="relative z-10 p-6 sm:p-8 h-full flex flex-col justify-between transition-transform duration-300 ease-out"
                            style="transform-style: preserve-3d;">
                            <div class="flex flex-col gap-3">
                                <h2 class="text-2xl font-bold text-white">Justify UI Kit</h2>
                                <p class="text-white/80 text-sm leading-relaxed">Modern, şık ve güçlü Vue.js
                                    bileşenleriyle projelerinizi bir üst seviyeye taşıyın.</p>
                            </div>
                            <div class="flex gap-3">
                                <div class="flex-1 bg-white/10 backdrop-blur-sm rounded-xl p-4 border border-white/10">
                                    <div class="text-xs text-white/70 uppercase tracking-wide">Components</div>
                                    <div class="text-2xl font-bold text-white">20+</div>
                                </div>
                                <div class="flex-1 bg-white/10 backdrop-blur-sm rounded-xl p-4 border border-white/10">
                                    <div class="text-xs text-white/70 uppercase tracking-wide">Variants</div>
                                    <div class="text-2xl font-bold text-white">50+</div>
                                </div>
                            </div>
                            <JButton variant="solid" @click="navigateToComponents"
                                class="w-full !py-3 !bg-slate-900 hover:!bg-slate-800 !rounded-xl !font-semibold transition-all duration-300">
                                Keşfet
                                <svg viewBox="0 0 24 24" fill="none" class="w-4 h-4">
                                    <path d="M14 5L21 12M21 12L14 19M21 12H3" stroke="currentColor" stroke-width="2"
                                        stroke-linecap="round" stroke-linejoin="round" />
                                </svg>
                            </JButton>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Pricing Section -->
            <section class="relative z-10 py-16 sm:py-20 px-4 sm:px-8">
                <div class="max-w-xl mx-auto">
                    <div class="text-center mb-12">
                        <h2 class="text-3xl sm:text-4xl font-bold text-slate-200 mb-3">Fiyatlandırma</h2>
                        <p class="text-lg text-slate-500">Hemen başlayın, tamamen ücretsiz.</p>
                    </div>

                    <JCard variant="glass" class="p-6 sm:p-8 text-center">
                        <JBadge variant="solid" class="mb-6">Ücretsiz</JBadge>

                        <div class="flex items-baseline justify-center gap-2 mb-2">
                            <span class="text-5xl sm:text-6xl font-extrabold text-slate-200">₺0</span>
                            <span class="text-lg text-slate-500">/ sonsuza dek</span>
                        </div>
                        <p class="text-slate-400 mb-8">Tüm bileşenlere sınırsız erişim</p>

                        <ul class="text-left space-y-4 mb-8">
                            <li class="flex items-center gap-3 text-slate-300 border-b border-slate-700/50 pb-4">
                                <svg viewBox="0 0 24 24" fill="none" class="w-5 h-5 text-green-500 flex-shrink-0">
                                    <path d="M5 13L9 17L19 7" stroke="currentColor" stroke-width="2"
                                        stroke-linecap="round" stroke-linejoin="round" />
                                </svg>
                                <span>20+ Vue.js Bileşeni</span>
                            </li>
                            <li class="flex items-center gap-3 text-slate-300 border-b border-slate-700/50 pb-4">
                                <svg viewBox="0 0 24 24" fill="none" class="w-5 h-5 text-green-500 flex-shrink-0">
                                    <path d="M5 13L9 17L19 7" stroke="currentColor" stroke-width="2"
                                        stroke-linecap="round" stroke-linejoin="round" />
                                </svg>
                                <span>50+ Varyant Seçeneği</span>
                            </li>
                            <li class="flex items-center gap-3 text-slate-300 border-b border-slate-700/50 pb-4">
                                <svg viewBox="0 0 24 24" fill="none" class="w-5 h-5 text-green-500 flex-shrink-0">
                                    <path d="M5 13L9 17L19 7" stroke="currentColor" stroke-width="2"
                                        stroke-linecap="round" stroke-linejoin="round" />
                                </svg>
                                <span>Kolay Özelleştirme</span>
                            </li>
                            <li class="flex items-center gap-3 text-slate-300 border-b border-slate-700/50 pb-4">
                                <svg viewBox="0 0 24 24" fill="none" class="w-5 h-5 text-green-500 flex-shrink-0">
                                    <path d="M5 13L9 17L19 7" stroke="currentColor" stroke-width="2"
                                        stroke-linecap="round" stroke-linejoin="round" />
                                </svg>
                                <span>TypeScript Desteği</span>
                            </li>
                            <li class="flex items-center gap-3 text-slate-300">
                                <svg viewBox="0 0 24 24" fill="none" class="w-5 h-5 text-green-500 flex-shrink-0">
                                    <path d="M5 13L9 17L19 7" stroke="currentColor" stroke-width="2"
                                        stroke-linecap="round" stroke-linejoin="round" />
                                </svg>
                                <span>Açık Kaynak</span>
                            </li>
                        </ul>

                        <JButton variant="solid" @click="navigateToComponents"
                            class="w-full !py-3 !bg-slate-600 hover:!bg-slate-500 !rounded-xl !font-semibold transition-all duration-300 hover:-translate-y-0.5">
                            Kullanmaya Başla
                            <svg viewBox="0 0 24 24" fill="none" class="w-5 h-5">
                                <path d="M14 5L21 12M21 12L14 19M21 12H3" stroke="currentColor" stroke-width="2"
                                    stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                        </JButton>
                    </JCard>
                </div>
            </section>

            <!-- FAQ Section -->
            <section class="relative z-10 py-12 sm:py-16 px-4 sm:px-8 pb-20">
                <div class="max-w-3xl mx-auto">
                    <div class="text-center mb-12">
                        <h2 class="text-3xl sm:text-4xl font-bold text-slate-200 mb-3">Sıkça Sorulan Sorular</h2>
                        <p class="text-lg text-slate-500">Merak ettiklerinizi yanıtlıyoruz.</p>
                    </div>

                    <div class="flex flex-col gap-3">
                        <div v-for="(faq, index) in faqs" :key="index"
                            class="bg-slate-800/50 border border-slate-700/50 rounded-xl overflow-hidden transition-all duration-300 hover:border-slate-600/50">
                            <button @click="toggleFaq(index)"
                                class="w-full flex items-center justify-between p-5 text-left text-slate-200 font-medium hover:text-white transition-colors duration-300">
                                <span>{{ faq.question }}</span>
                                <svg class="w-5 h-5 text-slate-500 transition-transform duration-300 flex-shrink-0"
                                    :class="{ 'rotate-180': faq.isOpen }" viewBox="0 0 24 24" fill="none">
                                    <path d="M6 9L12 15L18 9" stroke="currentColor" stroke-width="2"
                                        stroke-linecap="round" stroke-linejoin="round" />
                                </svg>
                            </button>
                            <div class="overflow-hidden transition-all duration-300"
                                :class="faq.isOpen ? 'max-h-40' : 'max-h-0'">
                                <p class="px-5 pb-5 text-slate-400 leading-relaxed">{{ faq.answer }}</p>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Footer -->
            <footer class="relative z-10 py-6 text-center text-slate-500 text-sm border-t border-slate-800">
                <p>© 2025 Justify UI Kit. Designed for developers.</p>
            </footer>
        </div>
    </MainLayout>
</template>
