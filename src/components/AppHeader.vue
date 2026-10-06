<script setup>
import { ref } from 'vue'
defineProps({ cartCount: { type: Number, default: 0 } })
const open = ref(false)
// Boutons purement visuels : pas de navigation pour l'instant.
const links = ['Accueil', 'Cartes anciennes', 'Nouveautés', 'Boutique', 'Contact']
</script>

<template>
  <header class="top">
    <span class="logo">🃏 Arcana Cards</span>
    <button class="burger" type="button" aria-label="Menu" :aria-expanded="open" @click="open = !open">☰</button>
    <nav :class="{ open }" aria-label="Navigation principale">
      <button v-for="l in links" :key="l" type="button" class="nav-btn" :class="{ active: l === 'Accueil' }">{{ l }}</button>
    </nav>
    <button class="cart" type="button" :aria-label="`Panier, ${cartCount} article${cartCount === 1 ? '' : 's'}`">🛒 Panier ({{ cartCount }})</button>
  </header>
</template>

<style scoped>
.top { position: sticky; top: 0; z-index: 10; display: flex; align-items: center; gap: 1rem; flex-wrap: nowrap; padding: .7rem max(1rem, calc((100vw - 1160px) / 2)); background: color-mix(in srgb, var(--surface) 94%, transparent); border-bottom: 1px solid var(--line); backdrop-filter: blur(14px); }
.logo { font-size: 1.3rem; font-weight: 700; color: var(--gold); margin-right: auto; }
nav { display: flex; gap: .3rem; flex-wrap: wrap; }
.nav-btn { background: none; border: 0; color: var(--text); padding: .5rem .8rem; border-radius: 8px; }
.nav-btn:hover { background: var(--line); }
.nav-btn.active { color: var(--gold); box-shadow: inset 0 -2px var(--gold); border-radius: 0; }
.cart { background: var(--gold); color: #111; border: 0; border-radius: 999px; padding: .5rem 1rem; font-weight: 700; }
.burger { display: none; background: none; border: 1px solid var(--line); color: var(--text); border-radius: 8px; padding: .4rem .7rem; font-size: 1.1rem; }
@media (max-width: 960px) {
  .burger { display: block; }
  nav { display: none; width: 100%; order: 5; flex-direction: column; }
  nav.open { display: flex; }
  .top { flex-wrap: wrap; gap: .5rem; padding-inline: .75rem; }
  .logo { font-size: 1.05rem; }
  .cart { padding: .45rem .7rem; font-size: .85rem; }
}
</style>
