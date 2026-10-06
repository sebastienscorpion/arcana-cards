<script setup>
import PlayingCard from './PlayingCard.vue'
defineProps({ item: { type: Object, required: true } })
defineEmits(['add'])
</script>

<template>
  <article class="prod fade">
    <div class="model-art" :style="{ '--accent': item.accent }" aria-hidden="true">
      <div class="model-back back-one"><span>{{ item.mark }}</span></div>
      <div class="model-back back-two"><span>{{ item.mark }}</span></div>
      <div class="model-face"><PlayingCard :rank="item.rank" :suit="item.suit" /></div>
    </div>
    <h3>{{ item.name }}</h3>
    <span class="tag">{{ item.tag }}</span>
    <p class="format">{{ item.format }}</p>
    <p class="description">{{ item.description }}</p>
    <span class="price">{{ item.price }}</span>
    <button class="btn alt buy" type="button" :aria-label="`Ajouter ${item.name} au panier`" @click="$emit('add', item)">Ajouter au panier</button>
  </article>
</template>

<style scoped>
.prod { background: var(--surface); border: 1px solid var(--line); border-radius: 8px; padding: 1.25rem; text-align: center; animation: fade .7s calc(var(--card-index, 0) * 100ms) both; transition: transform .25s, box-shadow .25s, border-color .25s; }
.prod:hover { transform: translateY(-6px); border-color: var(--gold); box-shadow: 0 14px 26px rgba(0, 0, 0, .2); }
.model-art { position: relative; width: 100%; height: 190px; margin: 0 auto .9rem; overflow: hidden; }
.model-back, .model-face { position: absolute; left: 50%; top: 50%; width: 102px; height: 144px; border-radius: 8px; }
.model-back { display: grid; place-items: center; border: 7px solid #f6f1e4; background: repeating-linear-gradient(45deg, transparent 0 5px, rgba(255,255,255,.16) 5px 6px), linear-gradient(145deg, color-mix(in srgb, var(--accent) 75%, white), var(--accent)); box-shadow: 0 7px 14px rgba(0,0,0,.24), inset 0 0 0 1px rgba(255,255,255,.5); color: rgba(255,255,255,.85); }
.model-back::before { position: absolute; inset: 5px; border: 1px solid rgba(255,255,255,.6); border-radius: 3px; content: ''; }
.model-back span { display: grid; place-items: center; width: 37px; aspect-ratio: 1; transform: rotate(45deg); border: 1px solid rgba(255,255,255,.72); font-family: Georgia, serif; font-size: 1.2rem; }
.model-back span::first-letter { transform: rotate(-45deg); }
.back-one { transform: translate(-74%, -48%) rotate(-16deg); }
.back-two { transform: translate(-27%, -51%) rotate(14deg); }
.model-face { transform: translate(-50%, -50%) rotate(-2deg); transition: transform .3s; }
.model-face :deep(.pc) { box-shadow: 0 8px 18px rgba(0,0,0,.35); }
.prod:hover .model-face { transform: translate(-50%, -52%) rotate(3deg) scale(1.04); }
h3 { min-height: 3rem; font-size: 1.05rem; line-height: 1.4; margin: .2rem 0 .4rem; }
.tag { display: inline-block; font-family: Arial, sans-serif; font-size: .7rem; font-weight: 700; padding: .2rem .6rem; border-radius: 99px; background: var(--line); }
.format { margin: .65rem 0 .25rem; font-family: Arial, sans-serif; font-size: .85rem; font-weight: 700; }
.description { min-height: 3.1rem; margin: 0; color: var(--muted); font-family: Arial, sans-serif; font-size: .85rem; line-height: 1.5; }
.price { display: block; margin: .8rem 0; font-weight: 700; color: var(--gold); font-size: 1.15rem; }
.buy { width: 100%; justify-content: center; font-size: .85rem; }
</style>
