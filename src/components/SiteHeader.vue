<script setup lang="ts">
defineProps<{ isMenuOpen: boolean }>()
defineEmits<{ toggleMenu: [] }>()
import logo from '@/assets/logo.png'


const navigation = ['دامنه', 'هاست', 'سرور', 'قیمت‌ها', 'امکانات', 'آموزش', 'درباره ما', 'تماس با ما']
</script>

<template>
  <header class="site-header">
    <div class="container header-inner">
      <a class="brand" href="#top" aria-label="سامان سرور">
        <img :src="logo" alt="سامان سرور" class="brand-logo" />
        <!-- <span class="brand-mark"><span></span><span></span><span></span></span> -->
      </a>
      <button class="menu-toggle" type="button" aria-label="باز کردن منو" @click="$emit('toggleMenu')">
        <span></span><span></span><span></span>
      </button>
      <nav class="main-nav" :class="{ 'is-open': isMenuOpen }">
        <a v-for="item in navigation" :key="item" href="#" @click="$emit('toggleMenu')">{{ item }}</a>
        <div class="mobile-actions">
<a class="login-link" href="#" @click.prevent="$emit('openAuth')">ورود</a>          <a class="primary-button small" href="#pricing">ثبت‌نام</a>
        </div>
      </nav>
      <div class="header-actions">
        <a class="login-link" href="#">ورود <span class="user-icon">♙</span></a>
        <a class="primary-button small" href="#" @click.prevent="$emit('openAuth')">ثبت‌نام</a>
      </div>
    </div>
  </header>
</template>
