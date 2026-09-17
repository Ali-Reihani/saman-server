<script setup lang="ts">
import { ref } from 'vue'

const selectedPlan = ref('استاندارد')
const plans = [
  { name: 'حرفه‌ای', icon: '◇', description: 'مناسب فروشگاه‌ها و سایت‌های پربازدید', price: '۱,۹۹۹,۰۰۰', features: ['۴۰ گیگابایت فضای SSD پرسرعت', 'پهنای باند نامحدود', 'نامحدود سایت', 'گواهی SSL رایگان'] },
  { name: 'استاندارد', icon: '♕', description: 'مناسب کسب‌وکارهای کوچک', price: '۱,۱۹۹,۰۰۰', features: ['۲۰ گیگابایت فضای SSD پرسرعت', 'پهنای باند نامحدود', '۵ سایت', 'گواهی SSL رایگان'] },
  { name: 'اقتصادی', icon: '➤', description: 'مناسب سایت‌های شخصی و وبلاگ‌ها', price: '۶۹۹,۰۰۰', features: ['۱۰ گیگابایت فضای SSD پرسرعت', 'پهنای باند نامحدود', '۱ سایت', 'گواهی SSL رایگان'] },
]
</script>

<template>
  <section id="pricing" class="pricing-section section-space">
    <div class="container">
      <div class="section-heading align-right">
        <span class="section-kicker">پلن‌های هاست اشتراکی</span>
        <h2>مناسب‌ترین انتخاب برای شروع</h2>
      </div>
      <div class="plans-grid">
        <article v-for="plan in plans" :key="plan.name" class="plan-card" :class="{ featured: plan.name === 'استاندارد', selected: selectedPlan === plan.name }" @click="selectedPlan = plan.name">
          <span v-if="plan.name === 'استاندارد'" class="popular-badge">پیشنهاد ویژه</span>
          <span class="plan-icon">{{ plan.icon }}</span>
          <h3>{{ plan.name }}</h3>
          <p class="plan-description">{{ plan.description }}</p>
          <div class="plan-price"><strong>{{ plan.price }}</strong><span>تومان / سالانه</span></div>
          <ul><li v-for="feature in plan.features" :key="feature">{{ feature }}</li></ul>
          <button class="plan-button" type="button">سفارش</button>
        </article>
      </div>
      <a class="all-plans" href="#pricing">مشاهده همه پلن‌ها <span>←</span></a>
    </div>
  </section>
</template>
