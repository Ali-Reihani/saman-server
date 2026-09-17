

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import logo from '@/assets/logo.png'


const emit = defineEmits<{ authed: [user: User] }>()

const mode = ref<'login' | 'signup'>('login')

const firstName = ref('')
const lastName = ref('')
const email = ref('')
const phone = ref('')
const password = ref('')
const passwordConfirm = ref('')
const captchaAnswer = ref('')
const captchaValue = ref('')
const captchaQuestion = ref('')
const acceptedTerms = ref(false)
const errors = ref({
  firstName: '',
  lastName: '',
  email: '',
  phone: '',
  password: '',
  passwordConfirm: '',
  captcha: '',
  terms: '',
})
const loading = ref(false)
const errorMsg = ref('')
const successMsg = ref('')


function generateCaptcha(): void {
  const a = Math.floor(Math.random() * 8) + 1
  const b = Math.floor(Math.random() * 8) + 1
  captchaValue.value = String(a + b)
  captchaQuestion.value = `${a} + ${b} = ؟`
  captchaAnswer.value = ''
}

onMounted(() => {
  generateCaptcha()
  supabase.auth.getSession().then(({ data }) => {
    if (data.session?.user) emit('authed', data.session.user)
  })
})

const isSignup = computed(() => mode.value === 'signup')

function switchMode(target: 'login' | 'signup'): void {
  mode.value = target
  errorMsg.value = ''
  successMsg.value = ''
  if (target === 'signup') generateCaptcha()
}

function validateLogin(): boolean {
  if (!email.value.includes('@')) {
    errorMsg.value = 'ایمیل معتبر وارد کنید.'
    return false
  }
  if (password.value.length < 6) {
    errorMsg.value = 'رمز عبور حداقل ۶ کاراکتر باشد.'
    return false
  }
  return true
}

function validateSignup(): boolean {
  errors.value = {
    firstName: '',
    lastName: '',
    email: '',
    phone: '',
    password: '',
    passwordConfirm: '',
    captcha: '',
    terms: '',
  }

  let valid = true

  if (!firstName.value.trim()) {
    errors.value.firstName = 'نام را وارد کنید.'
    valid = false
  }

  if (!lastName.value.trim()) {
    errors.value.lastName = 'نام خانوادگی را وارد کنید.'
    valid = false
  }

  if (!email.value.trim()) {
    errors.value.email = 'ایمیل را وارد کنید.'
    valid = false
  } else if (!email.value.includes('@')) {
    errors.value.email = 'ایمیل معتبر وارد کنید.'
    valid = false
  }

  if (!phone.value.trim()) {
    errors.value.phone = 'شماره همراه را وارد کنید.'
    valid = false
  } else if (!/^09\d{9}$/.test(phone.value.trim())) {
    errors.value.phone = 'شماره موبایل را به صورت 09xxxxxxxxx وارد کنید.'
    valid = false
  }

  if (!password.value) {
    errors.value.password = 'رمز عبور را وارد کنید.'
    valid = false
  } else if (password.value.length < 6) {
    errors.value.password = 'رمز عبور حداقل ۶ کاراکتر باشد.'
    valid = false
  }

  if (!passwordConfirm.value) {
    errors.value.passwordConfirm = 'تکرار رمز عبور را وارد کنید.'
    valid = false
  } else if (password.value !== passwordConfirm.value) {
    errors.value.passwordConfirm = 'تکرار رمز عبور یکسان نیست.'
    valid = false
  }

  if (!captchaAnswer.value.trim()) {
    errors.value.captcha = 'کد امنیتی را وارد کنید.'
    valid = false
  } else if (captchaAnswer.value !== captchaValue.value) {
    errors.value.captcha = 'کد امنیتی اشتباه است.'
    generateCaptcha()
    valid = false
  }

  if (!acceptedTerms.value) {
    errors.value.terms = 'پذیرش قوانین و شرایط استفاده الزامی است.'
    valid = false
  }

  return valid
}

async function handleSubmit(): Promise<void> {
  loading.value = true
  errorMsg.value = ''
  successMsg.value = ''

  if (isSignup.value) {
    if (!validateSignup()) {
      loading.value = false
      return
    }
    const { data, error } = await supabase.auth.signUp({
      email: email.value,
      password: password.value,
      options: {
        data: {
          first_name: firstName.value.trim(),
          last_name: lastName.value.trim(),
          phone: phone.value.trim(),
        },
      },
    })
    if (error) {
      errorMsg.value = translateError(error.message)
    } else if (data.user) {
      successMsg.value = 'حساب شما با موفقیت ساخته شد.'
      if (data.session) emit('authed', data.user)
    }
  } else {
    if (!validateLogin()) {
      loading.value = false
      return
    }
    const { data, error } = await supabase.auth.signInWithPassword({
      email: email.value,
      password: password.value,
    })
    if (error) {
      errorMsg.value = translateError(error.message)
    } else if (data.user) {
      emit('authed', data.user)
    }
  }

  loading.value = false
}

function translateError(msg: string): string {
  if (msg.includes('Invalid login')) return 'ایمیل یا رمز عبور اشتباه است.'
  if (msg.includes('already registered') || msg.includes('already been registered'))
    return 'این ایمیل قبلاً ثبت شده است. وارد شوید.'
  if (msg.includes('Email not confirmed')) return 'ایمیل تأیید نشده است.'
  return msg
}
</script>




<template>
  <section class="auth-section">
    <div class="auth-glow glow-a"></div>
    <div class="auth-glow glow-b"></div>

 <!-- سمت چپ: بخش چرخ دنده -->
  <div class="auth-visual">
    <div class="gear-circle">
      <div class="gear">⚙</div>
    </div>

    <div class="visual-text">
      <span>سامان سرور</span>
      <h2>زیرساختی قدرتمند<br />برای کسب‌وکار شما</h2>
      <p>
        سریع، امن و پایدار؛ همیشه در دسترس.
      </p>
    </div>
  </div>

  <!-- سمت راست: فرم فعلی -->
  <div class="auth-content">


    <div class="auth-card" :class="{ 'auth-card-wide': isSignup }">
      <div class="auth-brand">
                <img :src="logo" alt="سامان سرور" class="brand-logo" />
      </div>

      <div class="auth-tabs">
        <button type="button" :class="{ active: mode === 'login' }" @click="switchMode('login')">ورود</button>
        <button type="button" :class="{ active: mode === 'signup' }" @click="switchMode('signup')">ثبت‌نام</button>
      </div>

      <form @submit.prevent="handleSubmit" class="auth-form">
        <template v-if="isSignup">
          <div class="auth-row">
            <label>
                  <span>نام</span>

                  <input
                    v-model="firstName"
                    type="text"
                    placeholder="نام خود را وارد کنید"
                    autocomplete="given-name"
                  />

                  <small v-if="errors.firstName" class="field-error">
                    {{ errors.firstName }}
                  </small>
              </label>
            <label>
  <span>نام خانوادگی</span>

  <input
    v-model="lastName"
    type="text"
    placeholder="نام خانوادگی خود را وارد کنید"
    autocomplete="family-name"
  />

  <small v-if="errors.lastName" class="field-error">
    {{ errors.lastName }}
  </small>
</label>
          </div>
          <label>
  <span>آدرس ایمیل</span>

  <input
    v-model="email"
    type="email"
    placeholder="example@mail.com"
    autocomplete="email"
  />

  <small v-if="errors.email" class="field-error">
    {{ errors.email }}
  </small>
</label>
          <label>
  <span>شماره همراه</span>

  <input
    v-model="phone"
    type="tel"
    placeholder="09123456789 مثلا"
    autocomplete="tel"
  />

  <small v-if="errors.phone" class="field-error">
    {{ errors.phone }}
  </small>
</label>
          <div class="auth-row">
            <label>
  <span>رمز عبور</span>

  <input
    v-model="password"
    type="password"
    placeholder="حداقل ۶ کاراکتر"
    autocomplete="new-password"
  />

  <small v-if="errors.password" class="field-error">
    {{ errors.password }}
  </small>
</label>
            <label>
  <span>تکرار رمز عبور</span>

  <input
    v-model="passwordConfirm"
    type="password"
    placeholder="تکرار رمز عبور"
    autocomplete="new-password"
  />

  <small v-if="errors.passwordConfirm" class="field-error">
    {{ errors.passwordConfirm }}
  </small>
</label>
          </div>
          <label class="captcha-label">
  <span>کد امنیتی</span>

  <div class="captcha-wrap">
    <input
      v-model="captchaAnswer"
      type="text"
      inputmode="numeric"
      placeholder="جمع را وارد کنید"
    />

    <div
      class="captcha-box"
      @click="generateCaptcha"
      title="برای تغییر کد کلیک کنید"
    >
      <span>{{ captchaQuestion }}</span>
      <small class="change-pass">برای تغییر کلیک کنید</small>
    </div>
  </div>

  <small v-if="errors.captcha" class="field-error">
    {{ errors.captcha }}
  </small>
</label>
          <label class="terms-label">
  <div class="terms-content">
    <input type="checkbox" v-model="acceptedTerms" />

    <span>
      پذیرش
      <a href="#" @click.prevent>قوانین و شرایط استفاده</a>
    </span>
  </div>

  <small v-if="errors.terms" class="field-error">
    {{ errors.terms }}
  </small>
</label>
        </template>

        <template v-else>
          <label>
            <span>ایمیل</span>
            <input v-model="email" type="email" placeholder="example@mail.com" autocomplete="email" />
          </label>
          <label>
            <span>رمز عبور</span>
            <input v-model="password" type="password" placeholder="حداقل ۶ کاراکتر" autocomplete="current-password" />
          </label>
        </template>

        <p v-if="errorMsg" class="auth-error">{{ errorMsg }}</p>
        <p v-if="successMsg" class="auth-success">{{ successMsg }}</p>

        <button class="primary-button auth-submit" type="submit" :disabled="loading">
          {{ loading ? 'لطفاً صبر کنید...' : isSignup ? 'ساخت حساب' : 'ورود' }}
        </button>
      </form>

      <p class="auth-hint">
        {{ isSignup ? 'حساب دارید؟' : 'حساب ندارید؟' }}
        <a href="#" @click.prevent="switchMode(isSignup ? 'login' : 'signup')">
          {{ isSignup ? 'وارد شوید' : 'ثبت‌نام کنید' }}
        </a>
      </p>
    </div>
  </div>
  </section>
</template>
