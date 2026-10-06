<script setup>
const props = defineProps({ flip: Boolean })
const handKey = props.flip ? 'right' : 'left'

// Quatre doigts : position, largeur, sommet, inclinaison
const fingers = [
  { x: 50, w: 22, top: 40, r: -7 },
  { x: 73, w: 24, top: 22, r: -2 },
  { x: 98, w: 23, top: 32, r: 3 },
  { x: 121, w: 19, top: 56, r: 8 }
].map(f => ({
  ...f,
  cx: f.x + f.w / 2,
  creases: [0.38, 0.66].map(k => f.top + (118 - f.top) * k)
}))

function fingerPath(f) {
  const left = f.x
  const right = f.x + f.w
  const mid = f.cx
  const top = f.top
  return `M${left + f.w * .2} 122 C${left + f.w * .08} 97 ${left + f.w * .12} ${top + 18} ${left + f.w * .18} ${top + 9} Q${mid} ${top - 3} ${right - f.w * .18} ${top + 8} C${right - f.w * .08} ${top + 18} ${right - f.w * .08} 97 ${right - f.w * .2} 122 Z`
}
</script>

<template>
  <svg class="hand-svg" :class="{ flip }" viewBox="0 0 180 270" aria-hidden="true">
    <defs>
      <linearGradient :id="`skinH-${handKey}`" x1="0" x2="1"><stop offset="0" stop-color="#a96345"/><stop offset=".24" stop-color="#d99f79"/><stop offset=".56" stop-color="#f4c9a5"/><stop offset=".82" stop-color="#dfa982"/><stop offset="1" stop-color="#aa684e"/></linearGradient>
      <linearGradient :id="`skinV-${handKey}`" x1="0" y1="0" x2="1" y2=".3"><stop offset="0" stop-color="#c98e6b"/><stop offset=".48" stop-color="#f2c5a0"/><stop offset="1" stop-color="#c18461"/></linearGradient>
      <linearGradient :id="`nail-${handKey}`" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#ffeadf"/><stop offset="1" stop-color="#e8b39d"/></linearGradient>
      <linearGradient :id="`cuff-${handKey}`" x1="0" x2="1"><stop offset="0" stop-color="#d1d4d0"/><stop offset=".5" stop-color="#fff"/><stop offset="1" stop-color="#c4c8c4"/></linearGradient>
      <radialGradient :id="`glow-${handKey}`" cx=".4" cy=".3" r=".8"><stop offset="0" stop-color="#ffe2c7" stop-opacity=".8"/><stop offset="1" stop-color="#fbdcc2" stop-opacity="0"/></radialGradient>
      <filter :id="`soft-${handKey}`" x="-20%" y="-20%" width="140%" height="140%"><feDropShadow dx="0" dy="5" stdDeviation="4" flood-opacity=".38"/></filter>
    </defs>
    <g :filter="`url(#soft-${handKey})`">
      <path d="M44 248 L136 248 L150 270 L30 270 Z" fill="#26304f"/>
      <path d="M69 186 Q90 180 111 188 L113 239 Q91 246 68 239 Z" :fill="`url(#skinH-${handKey})`"/>
      <path d="M55 231 Q90 226 125 231 L127 250 Q91 255 53 250 Z" :fill="`url(#cuff-${handKey})`" stroke="#aeb2af" stroke-width="1"/>
      <circle cx="114" cy="242" r="3" fill="#b5b5b5"/>

      <g transform="rotate(-38 54 168)">
        <path d="M39 174 C32 162 34 132 37 111 Q39 97 49 96 Q60 96 63 109 L64 158 Q64 175 55 181 Q45 185 39 174 Z" :fill="`url(#skinH-${handKey})`"/>
        <path d="M40 119 Q40 101 50 101 Q59 102 59 119 Q50 124 40 119 Z" :fill="`url(#nail-${handKey})`" stroke="#d9a58f" stroke-width=".8"/>
        <path d="M38 140 Q49.5 146 61 140" fill="none" stroke="#a9704c" stroke-width="1.2" opacity=".55"/>
      </g>

      <g v-for="(f, i) in fingers" :key="i" :transform="`rotate(${f.r} ${f.cx} 118)`">
        <path :d="fingerPath(f)" :fill="`url(#skinH-${handKey})`"/>
        <path :d="`M${f.x + 4} ${f.top + 11} Q${f.cx} ${f.top + 4} ${f.x + f.w - 4} ${f.top + 11} L${f.x + f.w - 5} ${f.top + 23} Q${f.cx} ${f.top + 27} ${f.x + 5} ${f.top + 23} Z`" :fill="`url(#nail-${handKey})`" stroke="#d9a58f" stroke-width=".8"/>
        <ellipse :cx="f.cx - 2" :cy="f.top + 6" rx="2.5" ry="4" fill="#fff" opacity=".55"/>
        <path v-for="y in f.creases" :key="y" :d="`M${f.x + 3} ${y} Q${f.cx} ${y + 3} ${f.x + f.w - 3} ${y}`" fill="none" stroke="#a9704c" stroke-width="1.1" opacity=".5"/>
      </g>

      <path d="M51 107 Q47 96 58 93 Q74 89 90 94 Q111 88 132 94 Q146 98 144 112 L140 166 Q138 188 119 198 Q91 207 72 197 Q53 189 50 169 Z" :fill="`url(#skinV-${handKey})`"/>
      <path d="M51 107 Q47 96 58 93 Q74 89 90 94 Q111 88 132 94 Q146 98 144 112 L140 166 Q138 188 119 198 Q91 207 72 197 Q53 189 50 169 Z" :fill="`url(#glow-${handKey})`"/>
      <g fill="none" stroke="#a9704c" stroke-linecap="round" opacity=".4">
        <path d="M58 104 Q61 100 64 104"/><path d="M83 102 Q86 98 89 102"/><path d="M108 102 Q111 98 114 102"/><path d="M131 106 Q134 102 137 106"/>
        <path d="M70 118 Q76 150 82 188"/><path d="M96 118 Q96 150 96 190"/><path d="M122 118 Q116 150 110 188"/>
        <path d="M100 130 Q112 150 104 176" stroke="#7d8fb3" stroke-width="1.6" opacity=".5"/>
      </g>
    </g>
  </svg>
</template>

<style scoped>
.hand-svg { width: 100%; height: 100%; display: block; }
.flip { transform: scaleX(-1); }
</style>
