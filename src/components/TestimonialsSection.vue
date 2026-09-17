<script setup lang="ts">
const testimonials = [
  { name: 'رضا کریمی', role: 'صاحب سایت', quote: 'پشتیبانی خیلی سریع و حرفه‌ای بود. تجربه همکاری ما با سامان سرور عالی بوده است.', initials: 'رک' },
  { name: 'سارا یوسفی', role: 'مدیر فروشگاه آنلاین', quote: 'از وقتی سایتمون رو به سامان سرور منتقل کردیم، سرعت و امنیت خیلی بهتر شده.', initials: 'سی' },
  { name: 'علی محمدی', role: 'مدیر سایت آموزشی', quote: 'سرعت سرورها فوق‌العاده است و پشتیبانی همیشه در دسترس و حرفه‌ای هستند.', initials: 'عم' },
]
</script>

<template>
  <section class="testimonials-section section-space">
    <div class="container">
      <div class="section-heading align-right">
        <span class="section-kicker">نظرات مشتریان ما</span>
        <h2>اعتماد شما، سرمایه ماست.</h2>
      </div>
      <div class="testimonials-grid">
        <article v-for="item in testimonials" :key="item.name" class="testimonial-card">
          <span class="quote-mark">“</span>
          <p>{{ item.quote }}</p>
          <div class="person"><span class="avatar">{{ item.initials }}</span><span><b>{{ item.name }}</b><small>{{ item.role }}</small></span></div>
        </article>
      </div>
      <div class="logo-row" aria-label="همکاران تجاری"><b>cPanel</b><b>LiteSpeed</b><b>WordPress</b><b>CloudLinux</b><b>JetBackup</b></div>
    </div>
  </section>
</template>
