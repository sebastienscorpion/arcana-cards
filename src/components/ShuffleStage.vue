<script setup>
import { ref, computed, watch, onMounted, onBeforeUnmount } from 'vue'
import PlayingCard from './PlayingCard.vue'
import HandSvg from './HandSvg.vue'

const props = defineProps({ players: { type: Number, default: 2 } })

const phase = ref('shuffle') // 'shuffle' puis 'deal' en boucle
let timer

const cards = Array.from({ length: 12 }, (_, i) => ({
  i,
  sx: i % 2 ? 7 : -7,
  sr: i % 2 ? 12 : -12,
  ty: -(i * 0.15),
  er: ((i * 37) % 7) - 3 + 'deg'
}))

const seats = computed(() =>
  props.players === 3
    ? [{ x: -21, y: 3, n: 'Joueur 1' }, { x: 0, y: -13, n: 'Joueur 2' }, { x: 21, y: 3, n: 'Joueur 3' }]
    : [{ x: -21, y: 0, n: 'Joueur 1' }, { x: 21, y: 0, n: 'Joueur 2' }]
)

function cardStyle(c) {
  const s = seats.value[c.i % props.players]
  const k = Math.floor(c.i / props.players)
  const base = { '--i': c.i, '--sx': c.sx, '--sr': c.sr + 'deg', '--ty': c.ty, '--er': c.er }
  if (phase.value !== 'deal') return base
  return { ...base, transform: `translate(calc(var(--u)*${s.x + k * 0.6}), calc(var(--u)*${s.y + k * 0.4})) rotate(${k * 9 - 12}deg)` }
}

function cycle() {
  phase.value = 'shuffle'
  timer = setTimeout(() => { phase.value = 'deal'; timer = setTimeout(cycle, 3000) }, 4000)
}

onMounted(cycle)
onBeforeUnmount(() => clearTimeout(timer))
watch(() => props.players, () => { clearTimeout(timer); cycle() })
</script>

<template>
  <div class="stage" :class="phase" role="img" :aria-label="`Deux mains mélangent un paquet de cartes avant la distribution pour ${props.players} joueurs`">
    <div class="ring"></div>
    <span v-for="s in seats" :key="s.n" class="seat" :style="{ '--x': s.x, '--y': s.y + (s.y < -5 ? -8 : 8) }">{{ s.n }}</span>

    <div v-for="c in cards" :key="c.i" class="slot" :style="cardStyle(c)">
      <PlayingCard face-down />
    </div>

    <div class="hand" :class="{ away: phase === 'deal' }" style="--hx:-13; --hr:14; --sq:5">
      <div class="in"><HandSvg /></div>
    </div>
    <div class="hand" :class="{ away: phase === 'deal' }" style="--hx:13; --hr:-14; --sq:-5">
      <div class="in"><HandSvg flip /></div>
    </div>
  </div>
</template>

<style scoped>
.stage { --u: clamp(4px, 1.75cqw, 10px); position: relative; width: min(100%, calc(var(--u) * 56)); aspect-ratio: 1.3; margin: 0 auto; overflow: hidden; border-radius: calc(var(--u) * 2.4); border: calc(var(--u) * .55) solid #71502d;
  background:
    radial-gradient(ellipse at 50% 35%, rgba(255,255,255,.14), transparent 60%),
    url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='120' height='120'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='2'/><feColorMatrix values='0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 .12 0'/></filter><rect width='100%' height='100%' filter='url(%23n)'/></svg>"),
    radial-gradient(ellipse at center, var(--felt), var(--felt2));
  box-shadow: inset 0 0 40px rgba(0,0,0,.5), 0 18px 42px rgba(0,0,0,.27); }
.ring { position: absolute; inset: 8%; border: 2px dashed rgba(255,255,255,.16); border-radius: 50%; }
.shuffle .ring { animation: table-pulse 3s ease-in-out infinite alternate; }
@keyframes table-pulse { to { opacity: .45; transform: scale(.96); } }

.slot { position: absolute; left: 50%; top: 50%; width: calc(var(--u) * 8); height: calc(var(--u) * 11.5); margin: calc(var(--u) * -5.75) 0 0 calc(var(--u) * -4);
  transform: translate(0, calc(var(--u) * var(--ty))) rotate(var(--er)); }
.shuffle .slot { animation: riffle .65s cubic-bezier(.3,.7,.4,1) both; animation-delay: calc(var(--i) * .2s); }
.deal .slot { transition: transform .75s cubic-bezier(.2,.8,.3,1); transition-delay: calc(var(--i) * .1s); }
@keyframes riffle {
  from { transform: translate(calc(var(--u) * var(--sx)), calc(var(--u) * -1)) rotate(var(--sr)); }
  55%  { transform: translate(calc(var(--u) * var(--sx) * .3), calc(var(--u) * -4)) rotate(calc(var(--sr) * .3)); }
  to   { transform: translate(0, calc(var(--u) * var(--ty))) rotate(var(--er)); }
}

.hand { position: absolute; left: 50%; top: 50%; width: calc(var(--u) * 14); height: calc(var(--u) * 21); margin: calc(var(--u) * -10.5) 0 0 calc(var(--u) * -7); filter: drop-shadow(0 5px 8px rgba(0,0,0,.3));
  transform: translate(calc(var(--u) * var(--hx)), calc(var(--u) * 14.5)); transition: transform .6s ease, opacity .6s; }
.hand.away { transform: translate(calc(var(--u) * var(--hx)), calc(var(--u) * 34)); opacity: 0; }
.in { width: 100%; height: 100%; transform-origin: 50% 100%; }
.shuffle .in { animation: squeeze .52s ease-in-out infinite alternate; }
@keyframes squeeze {
  from { transform: rotate(calc(var(--hr) * 1deg)); }
  to   { transform: rotate(calc(var(--hr) * .35deg)) translateX(calc(var(--u) * var(--sq))); }
}

.seat { position: absolute; left: 50%; top: 50%; transform: translate(-50%, -50%) translate(calc(var(--u) * var(--x)), calc(var(--u) * var(--y)));
  font: 600 calc(var(--u) * 1.25)/1 Arial, sans-serif; color: rgba(255,255,255,.82); background: rgba(0,0,0,.25); padding: .45em .8em; border: 1px solid rgba(255,255,255,.15); border-radius: 99px; white-space: nowrap; transition: transform .4s; }
</style>
