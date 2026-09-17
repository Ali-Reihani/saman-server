<script setup lang="ts">
import { ref, onMounted } from 'vue'
import SiteHeader from './components/SiteHeader.vue'
import HeroSection from './components/HeroSection.vue'
import DomainSearch from './components/DomainSearch.vue'
import BenefitsSection from './components/BenefitsSection.vue'
import PricingSection from './components/PricingSection.vue'
import TestimonialsSection from './components/TestimonialsSection.vue'
import NewsletterBanner from './components/NewsletterBanner.vue'
import SiteFooter from './components/SiteFooter.vue'
import AuthPage from './components/AuthPage.vue'

const isMenuOpen = ref(false)
const showAuth = ref(false)
const currentUser = ref<User | null>(null)

function toggleMenu(): void {
  isMenuOpen.value = !isMenuOpen.value
}

function openAuth(): void {
  showAuth.value = true
}

function handleAuthed(user: User): void {
  currentUser.value = user
  showAuth.value = false
}

async function handleLogout(): Promise<void> {
  await supabase.auth.signOut()
  currentUser.value = null
}

onMounted(() => {
  supabase.auth.getSession().then(({ data }) => {
    if (data.session?.user) currentUser.value = data.session.user
  })

  supabase.auth.onAuthStateChange((_event, session) => {
    currentUser.value = session?.user ?? null
  })
})
</script>

<template>
  <AuthPage v-if="showAuth" @authed="handleAuthed" />
  <div v-else class="site-shell">
    <SiteHeader :is-menu-open="isMenuOpen" @toggle-menu="toggleMenu" @open-auth="openAuth" />
    <main>
      <HeroSection />
      <DomainSearch />
      <BenefitsSection />
      <PricingSection />
      <TestimonialsSection />
      <NewsletterBanner />
    </main>
    <SiteFooter />
  </div>

  <div v-if="currentUser" class="user-bar">
    <span>خوش آمدید، {{ currentUser.email }}</span>
    <button class="primary-button small" type="button" @click="handleLogout">خروج</button>
  </div>
</template>
