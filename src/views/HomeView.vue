<template>
  <!-- Navbar -->
  <nav class="navbar">
    <div class="navbar-container">
      <div class="logo">Tournoi Basket</div>
      <button
        class="mobile-nav-toggle"
        :class="{ active: mobileNavOpen }"
        aria-label="Toggle navigation"
        @click="toggleMobileNav"
        :aria-expanded="mobileNavOpen.toString()"
      >
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </button>
      <ul :class="['nav-links', { active: mobileNavOpen }]">
        <li><a @click.prevent="scrollToSection('home')">Accueil</a></li>
        <li><a @click.prevent="scrollToSection('rules')">Règles</a></li>
        <li><a @click.prevent="scrollToSection('register')">Inscription</a></li>
      </ul>
    </div>
    <div v-if="mobileNavOpen" class="overlay" @click="closeMobileNav"></div>
  </nav>

  <!-- Hero vidéo -->
  <section class="hero-video" data-aos="zoom-in">
    <video autoplay muted loop playsinline class="bg-video">
      <source src="/cross.mp4" type="video/mp4" />
      Ton navigateur ne supporte pas la vidéo.
    </video>
    <div class="hero-overlay ">
      <h1 class="hero-title section-title">Treg'One</h1>
      <p class="section-description">Ramène ton handle Repars avec ton cash</p>
    </div>
  </section>

  <!-- Sections -->
  <section id="home" data-aos="zoom-in-up" data-aos-offset="200" data-aos-easing="ease-in-sine">
    <div class="bg-effect bg-effect-1"></div>
    <div class="section-content">
      <h1 class="section-title">Treg'One</h1>
      <p class="section-description">Premier tournoi 1 vs 1 du département. Du shoot, du handle, du cross !</p>
      <p>Viens montrer que tu es le meilleur ! Du cash a gagner et le respect de tout le monde</p>
    </div>
  </section>

  <section id="rules" data-aos="zoom-in-up" data-aos-offset="200" data-aos-easing="ease-in-sine">
    <div class="bg-effect bg-effect-2"></div>
    <div class="section-content">
      <h1 class="section-title">Règles du tournoi</h1>
      <ul class="section-description list-disc list-inside">
        <ul>Matchs en 6 minutes ou 8 points</ul>
        <ul>1 / 2 Points par panier</ul>
        <ul>5 euros par personnes </ul>
        <ul>Le vainqueur repart avec la cagnotte</ul>
        <ul>Date et Horaires vont être communiqué ultérieurement</ul>
        <ul>Inscriptions dans la limite des places disponible</ul>
      </ul>
    </div>
  </section>

  <section id="register" data-aos="zoom-in-up" data-aos-offset="200" data-aos-easing="ease-in-sine">
    <div class="bg-effect bg-effect-1"></div>
    <div class="section-content">
      <h1 class="section-title">Inscription</h1>
      <button class="register-btn" @click="openModal">S'inscrire</button>
    </div>
  </section>

  <!-- Modal d'inscription -->
  <div v-if="isModalOpen" class="modal-overlay" @click.self="closeModal">
    <RegistrationForm />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import AOS from 'aos'
import 'aos/dist/aos.css'
import RegistrationForm from '../components/forms/RegistrationForm.vue'

const mobileNavOpen = ref(false)
const isModalOpen = ref(false)

function toggleMobileNav() {
  mobileNavOpen.value = !mobileNavOpen.value
}

function closeMobileNav() {
  mobileNavOpen.value = false
}

function openModal() {
  isModalOpen.value = true
}
function closeModal() {
  isModalOpen.value = false
}

function scrollToSection(id: string) {
  const el = document.getElementById(id)
  if (el) {
    el.scrollIntoView({ behavior: 'smooth' })
    closeMobileNav()
  }
}

onMounted(() => {
  AOS.init({ duration: 1200, once: false })
})
</script>

<style scoped>
/* Smooth scroll pour ancres */
html {
  scroll-behavior: smooth;
}

/* Navbar compacte */
.navbar {
  position: fixed;
  top: 0;
  width: 100%;
  padding: 0.6rem 5%;
  backdrop-filter: blur(12px);
  //background: var(--glass-bg);
  border-bottom: var(--border);
  z-index: 1000;
}
.navbar-container { /* inchangé */ }

/* Hero section */
.hero-video {
  position: relative;
  width: 100%;
  height: 40vh;
  overflow: hidden;
  margin-top: calc(0.6rem + 1px);
}
.bg-video {
  position: absolute;
  top: 50%;
  left: 50%;
  min-width: 100%;
  min-height: 100%;
  transform: translate(-50%, -50%);
  object-fit: cover;
  z-index: -1;
  filter: brightness(0.5) saturate(0.8); /* <-- Ajouté */
  opacity: 0.8; /* <-- Facultatif */

}

.hero-overlay {
  position: absolute; inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.6); /* ou même 0.6 */
  color: white;
  text-align: center;
}
.hero-title { font-size: 3rem; text-transform: uppercase; }
.hero-subtitle { font-size: 1.5rem; margin-top: 1rem; }

@keyframes bounce {
  0%,100% { transform: translateY(0); }
  50% { transform: translateY(10px); }
}

/* Sections & AOS */
section {
  min-height: 80vh;
  padding: 120px 5% 80px;
  position: relative;
}

.navbar.scrolled {
  padding: 0.8rem 5%;
  background: rgba(10, 10, 10, 0.95);
}

.navbar-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1400px;
  margin: 0 auto;
}

.logo {
  font-size: 1.8rem;
  font-weight: 700;
  background: var(--gradient);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: gradient 8s linear infinite;
  background-size: 300%;
  letter-spacing: -0.5px;
}

.nav-links {
  display: flex;
  gap: 2.5rem;
  list-style: none;
}

.nav-links a {
  color: white;
  text-decoration: none;
  font-weight: 500;
  position: relative;
  padding: 0.5rem 0;
  transition: all 0.3s ease;
  font-size: 1.05rem;
  letter-spacing: 0.3px;
}

.nav-links a::before {
  content: '';
  position: absolute;
  top: -4px;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--gradient);
  background-size: 300%;
  transition: width 0.3s ease;
}

.nav-links a::after {
  content: '';
  position: absolute;
  bottom: -4px;
  right: 0;
  width: 0;
  height: 2px;
  background: var(--gradient);
  background-size: 300%;
  transition: width 0.3s ease;
}

.nav-links a:hover {
  color: #fff;
  text-shadow: 0 0 8px rgba(255, 255, 255, 0.3);
}

.nav-links a:hover::before,
.nav-links a:hover::after {
  width: 100%;
  animation: gradient 8s linear infinite;
}

.mobile-nav-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  width: 40px;
  height: 40px;
  position: relative;
  z-index: 1001;
  border-radius: 50%;
  transition: background-color 0.3s ease;
}

.mobile-nav-toggle:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.mobile-nav-toggle .bar {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 20px;
  height: 2px;
  background: white;
  transition: all 0.4s ease;
}

.mobile-nav-toggle .bar:nth-child(1) { top: 14px; }
.mobile-nav-toggle .bar:nth-child(2) { top: 19px; }
.mobile-nav-toggle .bar:nth-child(3) { top: 24px; }

@keyframes gradient {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

@media (max-width: 768px) {
  .nav-links {
    position: fixed;
    top: 0;
    right: -100%;
    height: 100vh;
    width: 80%;
    max-width: 400px;
    background: linear-gradient(135deg, rgba(10, 10, 10, 0.99), rgba(20, 20, 20, 0.99));
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 2rem;
    transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: -10px 0 30px rgba(0, 0, 0, 0.5);
    padding: 2rem;
    backdrop-filter: blur(10px);
  }

  .nav-links::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.03));
    pointer-events: none;
  }

  .nav-links.active {
    right: 0;
  }

  .nav-links a {
    font-size: 1.2rem;
    opacity: 0;
    transform: translateX(20px);
    transition: all 0.4s ease;
  }

  .nav-links.active a {
    opacity: 1;
    transform: translateX(0);
  }

  .nav-links a:nth-child(1) { transition-delay: 0.1s; }
  .nav-links a:nth-child(2) { transition-delay: 0.2s; }
  .nav-links a:nth-child(3) { transition-delay: 0.3s; }
  .nav-links a:nth-child(4) { transition-delay: 0.4s; }
  .nav-links a:nth-child(5) { transition-delay: 0.5s; }

  .mobile-nav-toggle {
    display: block;
  }

  .mobile-nav-toggle.active .bar:nth-child(1) {
    transform: translate(-50%, 5px) rotate(45deg);
    width: 24px;
  }

  .mobile-nav-toggle.active .bar:nth-child(2) {
    opacity: 0;
  }

  .mobile-nav-toggle.active .bar:nth-child(3) {
    transform: translate(-50%, -5px) rotate(-45deg);
    width: 24px;
  }

  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    opacity: 0;
    visibility: hidden;
    transition: all 0.4s ease;
    backdrop-filter: blur(4px);
  }

  .overlay.active {
    opacity: 1;
    visibility: visible;
  }
}


.section-title {
  font-size: 3rem;
}

.section-description {
  font-size: 1rem;
  padding: 0 1rem;
}

section {
  min-height: 100vh;
  padding: 120px 5% 80px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
}

section:nth-child(even) {
  background: var(--section-bg);
}

.section-content {
  max-width: 1400px;
  width: 100%;
  text-align: center;
  position: relative;
  z-index: 1;
}

.section-title {
  font-size: 8vw;
  font-weight: 500;
  margin-bottom: 2rem;
  background: var(--gradient);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: gradient 8s linear infinite;
  background-size: 300%;
  line-height: 1.1;
  text-transform: uppercase;
  letter-spacing: -2px;
}

.section-description {
  font-size: 1.2rem;
  max-width: 800px;
  margin: 0 auto;
  line-height: 1.6;
  opacity: 0.8;
  color: white;
}

/* MODAL STYLES */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  backdrop-filter: blur(10px);
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
  animation: fadeIn 0.3s ease;
}

.modal-content {
  background-color: rgba(30, 30, 30, 0.95);
  padding: 2rem;
  border-radius: 1rem;
  position: relative;
  width: 90%;
  max-width: 600px;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.6);
  animation: slideUp 0.3s ease;
}

.close-button {
  position: absolute;
  top: -16px;
  right: -16px;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: none;
  border-radius: 50%;
  font-size: 1.2rem;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25);
  transition: background 0.3s ease, transform 0.2s ease;
  z-index: 10;
}

.close-button:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}


/* ANIMATIONS */
@keyframes fadeIn {
  from { opacity: 0 }
  to { opacity: 1 }
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.register-btn {
  background: var(--gradient);
  color: white;
  padding: 1rem 2rem;
  font-size: 1.2rem;
  border: none;
  border-radius: 9999px;
  cursor: pointer;
  margin-top: 1rem;
  transition: all 0.3s ease;
}

.register-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 0 15px rgba(255,255,255,0.3);
}
</style>
