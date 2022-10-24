<script setup>
</script>

<template>
  <main class="relative min-h-screen">
    <div class="fixed top-0 left-0 w-full h-full">
      <video ref="video" class="w-full h-full object-cover" autoplay muted>
        <source src="/fullhd.mp4">
      </video>
    </div>
    <div class="hero-wrapper opacity-90">
      <div class="hero">
        <canvas id="canv" width="32" height="20" />
        <h1 class="title glitch text-2xl lg:text-5xl xl:text-7xl" data-glitch="Morgenshtern - Last One">
          Morgenshtern - Last One
        </h1>
      </div>
      <div class="hero hero-behind flex flex-col pt-[100px]">
        <span class="md:text-lg desc md:w-[600px]" aria-hidden="true">
          LAST ONE — шестой полноформатный релиз Алишера, написанный после выезда артиста из России. Альбом должен стать
          последним на русском языке релизом, что ранее заявлял сам артист во всех возможных источниках.
        </span>
      </div>
    </div>
  </main>
  <footer>
    <div class="fixed bottom-[20px] left-[20px] text-white cursor-pointer" @click="mutedToggle">
      <svg v-if="muted" class="opacity-90" width="50" height="50" viewBox="0 0 24 24"><path fill="currentColor" d="M4.34 2.93L2.93 4.34L7.29 8.7L7 9H3v6h4l5 5v-6.59l4.18 4.18c-.65.49-1.38.88-2.18 1.11v2.06a8.94 8.94 0 0 0 3.61-1.75l2.05 2.05l1.41-1.41L4.34 2.93zM19 12c0 .82-.15 1.61-.41 2.34l1.53 1.53c.56-1.17.88-2.48.88-3.87c0-4.28-2.99-7.86-7-8.77v2.06c2.89.86 5 3.54 5 6.71zm-7-8l-1.88 1.88L12 7.76zm4.5 8A4.5 4.5 0 0 0 14 7.97v1.79l2.48 2.48c.01-.08.02-.16.02-.24z" /></svg>
      <svg v-else class="opacity-90" width="50" height="50" viewBox="0 0 24 24"><path fill="currentColor" d="M3 9v6h4l5 5V4L7 9H3zm13.5 3A4.5 4.5 0 0 0 14 7.97v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z" /></svg>
    </div>
  </footer>
</template>

<script setup>
import { onMounted, ref } from 'vue'

const muted = ref(true)
const video = ref(null)

const mutedToggle = () => {
  muted.value = !muted.value
  video.value.muted = muted.value
}

onMounted(() => {
  let c = document.getElementById('canv')
  let $ = c.getContext('2d')


  let col = function(x, y, r, g, b) {
    $.fillStyle = 'rgba(' + r + ',' + g + ',' + b + ', 0.90)'
    $.fillRect(x, y, 1,1)
  }

  let R = function(x, y, t) {
    return( Math.floor(192 + 64*Math.cos( (x*x-y*y)/300 + t )) )
  }

  let G = function(x, y, t) {
    return( Math.floor(192 + 64*Math.sin( (x*x*Math.cos(t/4)+y*y*Math.sin(t/3))/300 ) ) )
  }

  let B = function(x, y, t) {
    return( Math.floor(192 + 64*Math.sin( 5*Math.sin(t/9) + ((x-100)*(x-100)+(y-100)*(y-100))/1100) ))
  }

  let t = 0
  let fps = 60
  let run = function() {
    for(let x=0; x<=35; x++) {
      for(let y=0; y<=35; y++) {
        col(x, y, R(x,y,t), G(x,y,t), B(x,y,t))
      }
    }
    t = t + (0.05 /1)

    setTimeout(() => {
      window.requestAnimationFrame(run)
    }, 1000 / fps)

  }

  run()
})

</script>

<style lang="sass">
.vk
  @apply xl:fixed xl:w-[400px] xl:h-screen right-[0px] bottom-[0px] opacity-90
</style>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  width: 100%;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  margin: 0;
  height: 100vh;
  /*  */

}


.hero {
  height: 500px;
  position: relative;
  width: 100%;
  display: flex;
  padding: 0 20px;
  clip-path: polygon(0 0, 100% 0, 100% 40%, 0 100%);
}

@media (min-width: 1000px) {
  .hero {
    height: 300px;
    clip-path: polygon(0 0, 100% 0, 100% 25%, 0 100%);
  }
}

canvas {
  width: 100%;
  height: 100%;
  inset: 0;
  position: absolute;
}

.hero-wrapper {
  position: relative;
  display: flex;
  flex-direction: column;
}

.hero .title {
  position: relative;
  z-index: 2;
  width: 100%;
  color: #555;
  opacity: .9;
  margin-top: 20px;
  font-size: 4vw;
  mix-blend-mode: color-burn;
  font-weight: 900;
}

.desc {
  mix-blend-mode: color-burn;
  font-weight: 900;
  opacity: .9;
  color: #555;
}

.hero-behind {
  position: absolute !important;
  clip-path: none;
  top: 0;
}

.hero-behind .title {
  color: #111;
  opacity: 1;
  z-index: -1;
}

.glitch {
  position: relative;
  font-weight: 700;
  line-height: 1.2;
  color: #fff;
  letter-spacing: 5px;
  z-index: 1;
}

.glitch:before,
.glitch:after {
  display: block;
  content: attr(data-glitch);
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0.8;
}

.glitch:before {
  animation: glitch-color 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94) both infinite;
  color: #00ffff;
  z-index: -1;
}

.glitch:after {
  animation: glitch-color 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94) reverse both infinite;
  color: #ff00ff;
  z-index: -2;
}

@keyframes glitch-color {
  0% {
    transform: translate(0);
  }

  20% {
    transform: translate(-3px, 3px);
  }

  40% {
    transform: translate(-3px, -3px);
  }

  60% {
    transform: translate(3px, 3px);
  }

  80% {
    transform: translate(3px, -3px);
  }

  to {
    transform: translate(0);
  }
}
</style>
