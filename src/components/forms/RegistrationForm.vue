<template>
  <form
    action="https://formspree.io/f/manjdnvg"
    method="POST"
    @submit.prevent="handleSubmit"
    class="register-form"
  >
    <h2>Inscription</h2>

    <fieldset>
      <input v-model="firstName" type="text" name="firstName" placeholder="Prénom *" required />
      <p v-if="errors.firstName" class="error-message">{{ errors.firstName }}</p>

      <input
        v-model="age"
        type="number"
        min="1"
        max="120"
        name="age"
        placeholder="Âge *"
        required
      />
      <p v-if="errors.age" class="error-message">{{ errors.age }}</p>

      <input
        v-model="phone"
        type="tel"
        name="phone"
        placeholder="Numéro de téléphone *"
        pattern="^\+?\d{7,15}$"
        required
      />
      <p v-if="errors.phone" class="error-message">{{ errors.phone }}</p>

      <input
        v-model="email"
        type="email"
        name="email"
        placeholder="Email *"
        required
      />
      <p v-if="errors.email" class="error-message">{{ errors.email }}</p>
    </fieldset>

    <!-- Hidden field to help Formspree identify the form -->
    <input type="hidden" name="_subject" value="Nouvelle inscription" />
    <!-- Optional: pour redirection après inscription -->
    <input type="hidden" name="_next" value="https://ton-site.com/merci" />

    <button type="submit" class="btn" :disabled="loading">
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      S'inscrire
    </button>

    <p v-if="successMessage" class="success-message">{{ successMessage }}</p>
  </form>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from "axios";

const firstName = ref('')
const age = ref<number | null>(null)
const phone = ref('')
const email = ref('')

const errors = ref<Record<string, string>>({})
const successMessage = ref('')
const errorMessage = ref('')
const loading = ref(false)

function validate(): boolean {
  errors.value = {}

  if (!firstName.value) errors.value.firstName = 'Le prénom est requis.'
  if (!age.value || age.value < 1 || age.value > 120)
    errors.value.age = "L'âge doit être entre 1 et 120."
  if (!phone.value || !/^\+?\d{7,15}$/.test(phone.value))
    errors.value.phone = 'Numéro de téléphone invalide.'
  if (!email.value || !/^\S+@\S+\.\S+$/.test(email.value))
    errors.value.email = 'Email invalide.'

  return Object.keys(errors.value).length === 0
}

import { useToast } from 'vue-toastification'
import { useRouter } from 'vue-router'

const toast = useToast()
const router = useRouter()

async function handleSubmit() {
  if (!validate()) return

  successMessage.value = ''
  errorMessage.value = ''
  loading.value = true

  try {
    const formData = {
      firstName: firstName.value,
      age: age.value,
      phone: phone.value,
      email: email.value,
    }

    const res = await axios.post('http://localhost:3000/api/inscriptions', formData)

    successMessage.value = res.data.message
    toast.success(res.data.message)

    // Reset form
    firstName.value = ''
    age.value = null
    phone.value = ''
    email.value = ''

    // Redirection après un délai court pour laisser le toast visible
    setTimeout(() => {
      router.push('/')
    }, 1500)

  } catch (error: any) {
    if (axios.isAxiosError(error) && error.response) {
      const serverMessage = error.response.data?.message
      if (serverMessage) {
        errorMessage.value = serverMessage
        toast.error(serverMessage)
      } else {
        errorMessage.value = 'Une erreur est survenue, merci de réessayer.'
        toast.error(errorMessage.value)
      }
    }

  }
}


</script>

<style scoped>
.register-form {
  width: 400px;
  padding: 40px;
  margin: auto;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: rgba(0, 0, 0, 0.6);
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
  border-radius: 10px;
  box-sizing: border-box;
  text-align: center;
}

.register-form h2 {
  color: #fff;
  margin-bottom: 20px;
  font-size: 24px;
  letter-spacing: 1px;
}

.register-form fieldset {
  border: none;
  padding: 0;
  margin: 0;
}

.register-form input {
  width: 100%;
  padding: 10px 0;
  font-size: 14px;
  color: #fff;
  margin-bottom: 25px;
  border: none;
  border-bottom: 1px solid #fff;
  background: transparent;
  outline: none;
}

.error-message {
  color: #ff6b6b;
  font-size: 12px;
  margin-top: -20px;
  margin-bottom: 10px;
  text-align: left;
}

.success-message {
  color: #4caf50;
  font-size: 14px;
  margin-top: 20px;
}

.btn {
  position: relative;
  display: inline-block;
  padding: 10px 20px;
  color: #289bb8;
  font-size: 16px;
  text-decoration: none;
  overflow: hidden;
  transition: .5s;
  margin-top: 15px;
  letter-spacing: 2px;
  background: transparent;
  border: none;
  cursor: pointer;
}

.btn:hover {
  background: #289bb8;
  color: #fff;
  border-radius: 5px;
  box-shadow: 0 0 5px #289bb8,
  0 0 25px #289bb8,
  0 0 50px #289bb8,
  0 0 100px #289bb8;
}

.btn span {
  position: absolute;
  display: block;
}

.btn span:nth-child(1) {
  top: 0;
  left: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, #289bb8);
  animation: btn-anim1 1s linear infinite;
}

@keyframes btn-anim1 {
  0% { left: -100%; }
  50%, 100% { left: 100%; }
}

.btn span:nth-child(2) {
  top: -100%;
  right: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(180deg, transparent, #289bb8);
  animation: btn-anim2 1s linear infinite;
  animation-delay: .25s;
}

@keyframes btn-anim2 {
  0% { top: -100%; }
  50%, 100% { top: 100%; }
}

.btn span:nth-child(3) {
  bottom: 0;
  right: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(270deg, transparent, #289bb8);
  animation: btn-anim3 1s linear infinite;
  animation-delay: .5s;
}

@keyframes btn-anim3 {
  0% { right: -100%; }
  50%, 100% { right: 100%; }
}

.btn span:nth-child(4) {
  bottom: -100%;
  left: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(360deg, transparent, #289bb8);
  animation: btn-anim4 1s linear infinite;
  animation-delay: .75s;
}

@keyframes btn-anim4 {
  0% { bottom: -100%; }
  50%, 100% { bottom: 100%; }
}
</style>
