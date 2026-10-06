<script setup>
const links = ['Livraison', 'Conditions de vente', 'Confidentialité', 'Instagram', 'Newsletter']
</script>

<template>
  <footer>
    <div class="row">
      <button v-for="l in links" :key="l" type="button">{{ l }}</button>
    </div>
    <small>© 2026 Arcana Cards – Tous droits réservés</small>
  </footer>
</template>

<style scoped>
footer { border-top: 1px solid var(--line); background: var(--surface); padding: 1.5rem 1rem; text-align: center; }
.row { display: flex; flex-wrap: wrap; gap: .4rem; justify-content: center; margin-bottom: .8rem; }
button { background: none; border: 0; color: var(--text); padding: .5rem .8rem; border-radius: 8px; }
button:hover { background: var(--line); }
small { opacity: .7; }
</style>
