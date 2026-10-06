<script setup>
import { ref } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import ShuffleStage from './components/ShuffleStage.vue'
import ProductCard from './components/ProductCard.vue'
import { products } from './data/products.js'

const players = ref(2)
const cartCount = ref(0)

function addToCart() {
  cartCount.value += 1
}
</script>

<template>
  <AppHeader :cart-count="cartCount" />
  <main>
    <section class="hero">
      <div class="hero-copy">
        <p class="eyebrow">L’art du jeu, depuis 1760</p>
        <h1>Le jeu prend une autre allure.</h1>
        <p class="intro">Choisissez le jeu qui vous ressemble : tarot illustré, poker classique ou édition originale à offrir.</p>
        <div class="cta">
          <a class="btn" href="#boutique">Choisir un jeu <span aria-hidden="true">↗</span></a>
          <a class="text-link" href="#boutique">Comparer les modèles</a>
        </div>
      </div>
      <div class="hero-play">
        <div class="stage-shell">
          <ShuffleStage :players="players" />
        </div>
        <div class="play-controls">
          <span class="control-label">Autour de la table</span>
          <div class="seg" role="group" aria-label="Nombre de joueurs">
            <button type="button" :class="{ on: players === 2 }" :aria-pressed="players === 2" @click="players = 2">2 joueurs</button>
            <button type="button" :class="{ on: players === 3 }" :aria-pressed="players === 3" @click="players = 3">3 joueurs</button>
          </div>
        </div>
      </div>
    </section>
    <section id="boutique" class="collection">
      <div class="section-heading">
        <div>
          <p class="eyebrow">À découvrir</p>
          <h2>Nos modèles de cartes</h2>
        </div>
        <p class="section-note">Des jeux complets, chacun avec son format, son style et ses illustrations.</p>
      </div>
      <div class="grid">
        <ProductCard v-for="(p, index) in products" :key="p.id" :item="p" :style="{ '--card-index': index }" @add="addToCart" />
      </div>
    </section>
  </main>
  <AppFooter />
</template>

<style scoped>
main { width: min(100% - 2rem, 1160px); margin: 0 auto; padding: clamp(2rem, 6vw, 5rem) 0 4rem; }
.hero { display: grid; grid-template-columns: minmax(0, .86fr) minmax(0, 1.14fr); align-items: center; gap: clamp(2rem, 5vw, 5rem); min-height: 510px; }
.hero-copy { position: relative; z-index: 1; animation: rise-in .75s .08s both; }
.eyebrow { margin: 0 0 .85rem; color: var(--gold); font-family: Arial, sans-serif; font-size: .75rem; font-weight: 700; text-transform: uppercase; }
h1 { max-width: 10ch; margin: 0; font-size: clamp(2.6rem, 5.3vw, 5rem); line-height: .98; }
.intro { max-width: 34rem; margin: 1.3rem 0 0; color: var(--muted); font-family: Arial, sans-serif; font-size: 1.05rem; line-height: 1.7; }
.cta { display: flex; align-items: center; gap: 1.25rem; flex-wrap: wrap; margin-top: 2rem; }
.btn { display: inline-flex; align-items: center; gap: .7rem; text-decoration: none; }
.btn span { font-family: Arial, sans-serif; font-size: 1.1em; }
.text-link { color: var(--text); font-family: Arial, sans-serif; font-size: .9rem; text-underline-offset: .25em; }
.hero-play { min-width: 0; animation: rise-in .8s .2s both; }
.stage-shell { width: 100%; container-type: inline-size; }
.play-controls { display: flex; justify-content: space-between; align-items: center; gap: 1rem; margin: 1rem .35rem 0; }
.control-label { color: var(--muted); font-family: Arial, sans-serif; font-size: .85rem; }
.seg { display: inline-flex; flex: 0 0 auto; border: 1px solid var(--line); border-radius: 999px; padding: 3px; background: var(--surface); }
.seg button { min-height: 38px; background: none; border: 0; border-radius: 999px; color: var(--text); padding: .45rem .9rem; font-size: .85rem; }
.seg button.on { background: var(--gold); color: #111; font-weight: 700; }
.collection { margin-top: clamp(4rem, 9vw, 7rem); }
.section-heading { display: flex; justify-content: space-between; align-items: end; gap: 2rem; margin-bottom: 1.5rem; }
h2 { margin: 0; font-size: clamp(1.9rem, 3vw, 2.6rem); }
.section-note { max-width: 27rem; margin: 0; color: var(--muted); font-family: Arial, sans-serif; line-height: 1.6; }
.grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(min(100%, 220px), 1fr)); gap: clamp(1rem, 2.2vw, 1.6rem); }
@keyframes rise-in { from { opacity: 0; transform: translateY(18px); } to { opacity: 1; transform: translateY(0); } }
@media (max-width: 760px) {
  main { width: min(100% - 1.5rem, 560px); padding-top: 2.5rem; }
  .hero { grid-template-columns: 1fr; gap: 2rem; min-height: 0; }
  h1 { max-width: 11ch; font-size: clamp(2.8rem, 12vw, 4rem); }
  .hero-play { width: 100%; }
  .play-controls { margin-inline: 0; }
  .collection { margin-top: 4.5rem; }
  .section-heading { align-items: start; flex-direction: column; gap: .5rem; }
}
@media (max-width: 420px) {
  .play-controls { align-items: flex-start; flex-direction: column; }
  .cta { align-items: flex-start; flex-direction: column; gap: 1rem; }
}
</style>
