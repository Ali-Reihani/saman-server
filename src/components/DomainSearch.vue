<script setup lang="ts">
import { ref } from 'vue'

const query = ref('')
const searched = ref(false)
const extensions = [
  { name: '.com.ir', price: '۲۹,۰۰۰' },
  { name: '.com', price: '۳۹,۰۰۰' },
  { name: '.org', price: '۴۹,۰۰۰' },
  { name: '.net', price: '۶۹,۰۰۰' },
  { name: '.ir', price: '۱۹,۰۰۰' },
]

function searchDomain(): void {
  searched.value = query.value.trim().length > 0
}
</script>

<template>
  <section class="domain-section" aria-labelledby="domain-title">
    <div class="container domain-card">
      <div class="domain-intro">
        <h2 id="domain-title">ثبت دامنه</h2>
        <p>{{ searched ? `نتیجه جست‌وجو برای «${query}»` : 'نام دامنه مناسب خود را پیدا کنید' }}</p>
      </div>
      <form class="search-box" @submit.prevent="searchDomain">
        <input v-model="query" type="text" placeholder="نام دامنه مورد نظر خود را وارد کنید..." aria-label="نام دامنه" />
        <button class="primary-button" type="submit">جست‌وجو</button>
      </form>
      <div class="extension-list">
        <div v-for="extension in extensions" :key="extension.name" class="extension-item">
          <b>{{ extension.name }}</b><span>از {{ extension.price }} تومان</span>
        </div>
      </div>
    </div>
  </section>
</template>
