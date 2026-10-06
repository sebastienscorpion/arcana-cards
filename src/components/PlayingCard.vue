<script setup>
import { computed } from 'vue'
const props = defineProps({
  rank: { type: String, default: 'A' },
  suit: { type: String, default: 's' },      // s = pique, h = coeur, d = carreau, c = trèfle
  faceDown: Boolean,
  old: Boolean                                // aspect ancien (papier jauni)
})
const symbols = { s: '♠', h: '♥', d: '♦', c: '♣' }
const symbol = computed(() => symbols[props.suit])
const red = computed(() => props.suit === 'h' || props.suit === 'd')
const court = computed(() => ['J', 'Q', 'K'].includes(props.rank))
const count = computed(() => (props.rank === 'A' ? 1 : Number(props.rank) || 0))
const pipSize = computed(() => (count.value === 1 ? '58cqw' : count.value <= 3 ? '30cqw' : '22cqw'))
</script>

<template>
  <div class="pc" :class="{ back: faceDown, old }">
    <div v-if="faceDown" class="back-art"></div>
    <template v-else>
      <div class="idx tl" :class="red ? 'r' : 'k'"><b>{{ rank }}</b><i>{{ symbol }}</i></div>
      <div class="idx br" :class="red ? 'r' : 'k'"><b>{{ rank }}</b><i>{{ symbol }}</i></div>
      <div v-if="court" class="mid court" :class="red ? 'r' : 'k'">
        <div class="frame"><span class="letter">{{ rank }}</span><span class="s">{{ symbol }}</span></div>
      </div>
      <div v-else class="mid" :class="red ? 'r' : 'k'" :style="{ fontSize: pipSize }">
        <span v-for="n in count" :key="n">{{ symbol }}</span>
      </div>
    </template>
  </div>
</template>

<style scoped>
.pc { position: relative; width: 100%; height: 100%; container-type: inline-size; border-radius: 7% / 5%; background: linear-gradient(145deg, #fff, #f1ebdb); box-shadow: inset 0 0 0 1px rgba(0,0,0,.1), 0 3px 7px rgba(0,0,0,.4); font-family: Georgia, serif; user-select: none; overflow: hidden; }
.pc.old { background: linear-gradient(145deg, #f1e2b8, #dcc48c); filter: sepia(.45) contrast(.95); }
.idx { position: absolute; left: 8%; top: 5%; display: flex; flex-direction: column; align-items: center; line-height: 1; font-size: 24cqw; }
.idx i { font-style: normal; font-size: 21cqw; }
.br { left: auto; top: auto; right: 8%; bottom: 5%; transform: rotate(180deg); }
.r { color: #c1121f; } .k { color: #14151a; }
.mid { position: absolute; inset: 18% 22%; display: flex; flex-wrap: wrap; align-content: center; justify-content: center; gap: 1cqw; line-height: 1; }
.frame { width: 100%; height: 100%; border: 1.5px solid currentColor; border-radius: 4%; display: flex; flex-direction: column; align-items: center; justify-content: center; background: repeating-linear-gradient(45deg, rgba(0,0,0,.04) 0 3px, transparent 3px 6px); }
.letter { font-size: 44cqw; font-weight: 700; line-height: 1; }
.s { font-size: 24cqw; line-height: 1; }
.back { background: #fdfdfb; padding: 6%; }
.back-art { position: relative; width: 100%; height: 100%; border-radius: 5%; border: 1px solid #55101a;
  background: repeating-linear-gradient(45deg, rgba(255,255,255,.2) 0 1px, transparent 1px 9px), repeating-linear-gradient(-45deg, rgba(255,255,255,.2) 0 1px, transparent 1px 9px), linear-gradient(160deg, #a3212f, #6b1120);
  box-shadow: inset 0 0 0 3px rgba(255,255,255,.18); }
.back-art::after { content: ''; position: absolute; left: 50%; top: 50%; width: 34%; aspect-ratio: 1; transform: translate(-50%, -50%) rotate(45deg); border: 1px solid #e8c766; background: radial-gradient(circle, #e8c766 0 18%, #8b1a28 20%); }
</style>
