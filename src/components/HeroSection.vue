<template>
  <section id="top" class="hero-section">
    <div class="hero-glow glow-one"></div>
    <div class="hero-glow glow-two"></div>
    <div class="container hero-content">
      <div class="hero-copy">
        <span class="eyebrow">هاستینگ مطمئن برای کسب‌وکارهای آنلاین</span>
        <h1>سایتی که هرگز<br /><strong>نمی‌خوابد!</strong></h1>
        <p>سرورهای پرسرعت، امنیت پیشرفته و پشتیبانی واقعی<br class="desktop-only" /> تا خیال شما از میزبانی راحت باشد.</p>
        <div class="hero-actions">
          <a class="primary-button" href="#pricing">مشاهده پلن‌ها <span>←</span></a>
          <a class="outline-button" href="#contact">مشاوره رایگان <span>◌</span></a>
        </div>
        <div class="hero-stats">
          <div class="stat"><span class="stat-icon">↯</span><span><b>سرعت فوق‌العاده</b><small>NVMe و SSD پرسرعت</small></span></div>
          <div class="stat"><span class="stat-icon">◈</span><span><b>آپتایم ۹۹.۹٪</b><small>تضمین در دسترس بودن</small></span></div>
          <div class="stat"><span class="stat-icon">♧</span><span><b>پشتیبانی ۲۴/۷</b><small>پاسخگویی در کمتر از ۱۰ دقیقه</small></span></div>
        </div>
      </div>
      <div class="server-art" aria-label="تصویر سرورهای امن">
        <div class="server-cloud cloud-left"></div>
        <div class="server-cloud cloud-right"></div>
        <div class="server-stack stack-back"><span v-for="n in 5" :key="n"></span></div>
        <div class="server-stack stack-mid"><span v-for="n in 6" :key="n"></span></div>
        <div class="server-stack stack-front"><span v-for="n in 7" :key="n"></span></div>
        <div class="shield"><i>✓</i></div>
      </div>
    </div>
  </section>
</template>
