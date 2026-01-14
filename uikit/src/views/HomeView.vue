<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { MainLayout } from '@/layouts'
import { JButton } from '@/components'

const router = useRouter()

// Tilt Card Refs
const cardRef = ref<HTMLElement | null>(null)
const glowRef = ref<HTMLElement | null>(null)
const contentRef = ref<HTMLElement | null>(null)

const navigateToComponents = () => {
    router.push('/components')
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

// Animated Background Particles
const particles = ref<Array<{ id: number; x: number; y: number; size: number; duration: number; delay: number }>>([])

onMounted(() => {
    for (let i = 0; i < 30; i++) {
        particles.value.push({
            id: i,
            x: Math.random() * 100,
            y: Math.random() * 100,
            size: Math.random() * 3 + 1,
            duration: Math.random() * 20 + 15,
            delay: Math.random() * 10
        })
    }
})
</script>

<template>
    <MainLayout title="Justify UI" :show-hero="false" :show-footer="false" navbar-variant="home" container-width="full">
        <div class="home-content">
            <!-- Animated Background -->
            <div class="particles-bg">
                <div v-for="particle in particles" :key="particle.id" class="particle" :style="{
                    left: `${particle.x}%`,
                    top: `${particle.y}%`,
                    width: `${particle.size}px`,
                    height: `${particle.size}px`,
                    animationDuration: `${particle.duration}s`,
                    animationDelay: `${particle.delay}s`
                }" />
            </div>

            <!-- Gradient Overlay -->
            <div class="gradient-overlay"></div>

            <!-- Hero Content -->
            <div class="hero-container">
                <!-- Left Side - Text Content -->
                <div class="hero-text">
                    <h1 class="hero-title">
                        <span class="title-gradient">Justify UI Kit</span>
                        <br />
                        <span class="title-secondary">Modern Components</span>
                    </h1>

                    <p class="hero-description">
                        Yeni nesil web uygulamalarınız için tasarlanmış, zarif ve güçlü Vue.js bileşen kütüphanesi.
                        Hızlı geliştirme, tutarlı tasarım ve mükemmel kullanıcı deneyimi için oluşturuldu.
                    </p>

                    <div class="hero-features">
                        <div class="feature-item">
                            <svg class="feature-icon" viewBox="0 0 24 24" fill="none">
                                <path
                                    d="M9 12L11 14L15 10M21 12C21 16.9706 16.9706 21 12 21C7.02944 21 3 16.9706 3 12C3 7.02944 7.02944 3 12 3C16.9706 3 21 7.02944 21 12Z"
                                    stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                    stroke-linejoin="round" />
                            </svg>
                            <span>20+ Components</span>
                        </div>
                        <div class="feature-item">
                            <svg class="feature-icon" viewBox="0 0 24 24" fill="none">
                                <path d="M13 10V3L4 14H11V21L20 10H13Z" stroke="currentColor" stroke-width="2"
                                    stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                            <span>Blazing Fast</span>
                        </div>
                        <div class="feature-item">
                            <svg class="feature-icon" viewBox="0 0 24 24" fill="none">
                                <path d="M4 6H20M4 12H20M4 18H20" stroke="currentColor" stroke-width="2"
                                    stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                            <span>Multiple Variants</span>
                        </div>
                    </div>

                    <div class="hero-buttons">
                        <JButton variant="solid" class="btn-hero-primary" @click="navigateToComponents">
                            <span>Bileşenleri Keşfet</span>
                            <svg class="btn-icon" viewBox="0 0 24 24" fill="none">
                                <path d="M14 5L21 12M21 12L14 19M21 12H3" stroke="currentColor" stroke-width="2"
                                    stroke-linecap="round" stroke-linejoin="round" />
                            </svg>
                        </JButton>
                        <JButton variant="outline" class="btn-hero-secondary"
                            @click="() => window.open('https://github.com/SynkennL/JustifyyUIKit', '_blank')">
                            <svg class="btn-icon-left" fill="currentColor" viewBox="0 0 24 24">
                                <path fill-rule="evenodd"
                                    d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z"
                                    clip-rule="evenodd" />
                            </svg>
                            <span>GitHub</span>
                        </JButton>
                    </div>
                </div>

                <!-- Right Side - Tilt Card -->
                <div class="hero-card-container">
                    <div ref="cardRef" class="tilt-card" @mousemove="handleMouseMove" @mouseleave="handleMouseLeave">
                        <div ref="glowRef" class="card-glow"></div>
                        <div ref="contentRef" class="card-content">
                            <div class="card-header">
                                <h2 class="card-title">Justify UI Kit</h2>
                                <p class="card-description">Modern, şık ve güçlü Vue.js bileşenleriyle projelerinizi bir
                                    üst seviyeye taşıyın.</p>
                            </div>
                            <div class="card-stats">
                                <div class="stat-item">
                                    <div class="stat-label">Components</div>
                                    <div class="stat-value">20+</div>
                                </div>
                                <div class="stat-item">
                                    <div class="stat-label">Variants</div>
                                    <div class="stat-value">50+</div>
                                </div>
                            </div>
                            <JButton variant="solid" class="card-button" @click="navigateToComponents">
                                Keşfet
                                <svg viewBox="0 0 24 24" fill="none" class="w-4 h-4 ml-2">
                                    <path d="M14 5L21 12M21 12L14 19M21 12H3" stroke="currentColor" stroke-width="2"
                                        stroke-linecap="round" stroke-linejoin="round" />
                                </svg>
                            </JButton>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Footer -->
            <footer class="home-footer">
                <p>© 2025 Justify UI Kit. Designed for developers.</p>
            </footer>
        </div>
    </MainLayout>
</template>

<style scoped>
.home-content {
    position: relative;
    min-height: calc(100vh - 56px);
    display: flex;
    flex-direction: column;
    margin: -2.5rem -1.5rem;
}

/* Particles Background */
.particles-bg {
    position: absolute;
    inset: 0;
    overflow: hidden;
    pointer-events: none;
}

.particle {
    position: absolute;
    background: rgba(100, 116, 139, 0.4);
    border-radius: 50%;
    animation: float linear infinite;
}

@keyframes float {

    0%,
    100% {
        transform: translateY(0) translateX(0);
        opacity: 0;
    }

    10% {
        opacity: 1;
    }

    90% {
        opacity: 1;
    }

    100% {
        transform: translateY(-100vh) translateX(20px);
        opacity: 0;
    }
}

/* Gradient Overlay */
.gradient-overlay {
    position: absolute;
    inset: 0;
    background:
        radial-gradient(ellipse at 20% 0%, rgba(71, 85, 105, 0.15) 0%, transparent 50%),
        radial-gradient(ellipse at 80% 100%, rgba(51, 65, 85, 0.1) 0%, transparent 50%);
    pointer-events: none;
}

/* Hero Container */
.hero-container {
    flex: 1;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
    max-width: 1280px;
    margin: 0 auto;
    padding: 4rem 2rem;
    position: relative;
    z-index: 10;
}

/* Hero Text */
.hero-text {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}

.hero-title {
    font-size: 3.5rem;
    line-height: 1.1;
    font-weight: 800;
}

.title-gradient {
    background: linear-gradient(135deg, #e2e8f0 0%, #94a3b8 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

.title-secondary {
    color: #64748b;
}

.hero-description {
    font-size: 1.125rem;
    color: #94a3b8;
    line-height: 1.8;
    max-width: 500px;
}

.hero-features {
    display: flex;
    flex-wrap: wrap;
    gap: 1.5rem;
}

.feature-item {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    color: #94a3b8;
    font-size: 0.875rem;
}

.feature-icon {
    width: 1.25rem;
    height: 1.25rem;
    color: #64748b;
}

.hero-buttons {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    margin-top: 0.5rem;
}

/* Custom Button Styles for Hero */
.btn-hero-primary {
    display: inline-flex !important;
    align-items: center;
    gap: 0.75rem;
    padding: 1rem 2rem !important;
    border-radius: 0.75rem !important;
    font-weight: 600 !important;
    font-size: 1rem;
    background: #475569 !important;
    transition: all 0.3s ease !important;
}

.btn-hero-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 20px 40px -10px rgba(71, 85, 105, 0.4);
    background: #64748b !important;
}

.btn-icon {
    width: 1.25rem;
    height: 1.25rem;
    transition: transform 0.3s ease;
}

.btn-hero-primary:hover .btn-icon {
    transform: translateX(4px);
}

.btn-hero-secondary {
    display: inline-flex !important;
    align-items: center;
    gap: 0.5rem;
    padding: 1rem 1.5rem !important;
    border-radius: 0.75rem !important;
    font-weight: 600 !important;
    font-size: 1rem;
    border-color: #475569 !important;
    transition: all 0.3s ease !important;
}

.btn-hero-secondary:hover {
    background: rgba(71, 85, 105, 0.3) !important;
}

.btn-icon-left {
    width: 1.25rem;
    height: 1.25rem;
}

/* Tilt Card */
.hero-card-container {
    display: flex;
    justify-content: center;
    align-items: center;
    perspective: 1000px;
}

.tilt-card {
    width: 340px;
    height: 420px;
    background: linear-gradient(135deg, #334155 0%, #475569 50%, #64748b 100%);
    border-radius: 1.5rem;
    box-shadow:
        0 25px 50px -12px rgba(0, 0, 0, 0.4),
        0 0 0 1px rgba(148, 163, 184, 0.1) inset;
    position: relative;
    cursor: pointer;
    transition: all 0.3s ease-out;
    transform-style: preserve-3d;
}

.tilt-card:hover {
    box-shadow:
        0 35px 60px -15px rgba(0, 0, 0, 0.5),
        0 0 0 1px rgba(148, 163, 184, 0.2) inset;
}

.card-glow {
    position: absolute;
    inset: 0;
    border-radius: 1.5rem;
    opacity: 0;
    transition: opacity 0.3s ease;
    pointer-events: none;
}

.card-content {
    position: relative;
    z-index: 10;
    padding: 2rem;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: transform 0.3s ease-out;
    transform-style: preserve-3d;
}

.card-header {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
}

.card-title {
    font-size: 1.75rem;
    font-weight: 700;
    color: white;
}

.card-description {
    color: rgba(255, 255, 255, 0.8);
    font-size: 0.9rem;
    line-height: 1.6;
}

.card-stats {
    display: flex;
    gap: 1rem;
}

.stat-item {
    flex: 1;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(8px);
    border-radius: 0.75rem;
    padding: 1rem;
    border: 1px solid rgba(255, 255, 255, 0.1);
}

.stat-label {
    font-size: 0.75rem;
    color: rgba(255, 255, 255, 0.7);
    text-transform: uppercase;
    letter-spacing: 0.05em;
}

.stat-value {
    font-size: 1.5rem;
    font-weight: 700;
    color: white;
}

.card-button {
    display: flex !important;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    width: 100%;
    padding: 0.875rem !important;
    background: #1e293b !important;
    color: #e2e8f0 !important;
    font-weight: 600 !important;
    border-radius: 0.75rem !important;
    transition: all 0.3s ease !important;
}

.card-button:hover {
    transform: scale(1.02);
    background: #334155 !important;
}

/* Footer */
.home-footer {
    position: relative;
    z-index: 10;
    padding: 2rem;
    text-align: center;
    color: #475569;
    font-size: 0.875rem;
    border-top: 1px solid rgba(71, 85, 105, 0.2);
}

/* Responsive */
@media (max-width: 1024px) {
    .hero-container {
        grid-template-columns: 1fr;
        gap: 3rem;
        text-align: center;
    }

    .hero-text {
        align-items: center;
    }

    .hero-description {
        max-width: 100%;
    }

    .hero-features {
        justify-content: center;
    }

    .hero-buttons {
        justify-content: center;
    }

    .hero-title {
        font-size: 2.5rem;
    }
}

@media (max-width: 640px) {
    .hero-container {
        padding: 2rem 1rem;
    }

    .hero-title {
        font-size: 2rem;
    }

    .tilt-card {
        width: 300px;
        height: 380px;
    }
}
</style>
