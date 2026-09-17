<script setup lang="ts">
import { ref } from 'vue'

const email = ref('')
const subscribed = ref(false)

function subscribe(): void {
  if (email.value.includes('@')) subscribed.value = true
}
</script>

<template>
  <section class="newsletter-section">
    <div class="container newsletter-card">
      <div class="newsletter-art"><span></span><span></span><span></span></div>
      <div class="newsletter-content">
        <h2>از تخفیف‌ها و اخبار ویژه ما خبر شوید!</h2>
        <p>ایمیل خود را وارد کنید تا بهترین پیشنهادها را دریافت کنید.</p>
        <form class="newsletter-form" @submit.prevent="subscribe">
          <input v-model="email" type="email" placeholder="ایمیل شما" aria-label="ایمیل شما" />
          <button class="primary-button" type="submit">ثبت‌نام</button>
        </form>
        <span v-if="subscribed" class="success-message">ایمیل شما با موفقیت ثبت شد.</span>
      </div>
    </div>
  </section>
</template>
