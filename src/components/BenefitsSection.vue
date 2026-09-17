<script setup lang="ts">
const benefits = [
  { icon: '◇', title: 'ضمانت بازگشت وجه', text: 'تا ۳۰ روز ضمانت بازگشت وجه بدون هیچ سوالی' },
  { icon: '☁', title: 'بکاپ روزانه', text: 'تهیه نسخه پشتیبان خودکار برای آرامش خیال شما' },
  { icon: '◌', title: 'پشتیبانی واقعی', text: 'تیم متخصص ما در کمتر از ۱۰ دقیقه پاسخگو است' },
  { icon: '⌁', title: 'سرعت بی‌نظیر', text: 'استفاده از بهترین زیرساخت برای تجربه‌ای سریع' },
  { icon: '♢', title: 'امنیت پیشرفته', text: 'فایروال اختصاصی و محافظت همیشگی در برابر تهدیدها' },
]
</script>

<template>
  <section class="benefits-section section-space">
    <div class="container">
      <div class="section-heading align-right">
        <span class="section-kicker">چرا سامان سرور؟</span>
        <h2>چون فقط هاست نمی‌فروشیم، آرامش می‌دهیم!</h2>
      </div>
      <div class="benefits-grid">
        <article v-for="benefit in benefits" :key="benefit.title" class="benefit-card">
          <span class="feature-icon">{{ benefit.icon }}</span>
          <h3>{{ benefit.title }}</h3>
          <p>{{ benefit.text }}</p>
        </article>
      </div>
    </div>
  </section>
</template>
