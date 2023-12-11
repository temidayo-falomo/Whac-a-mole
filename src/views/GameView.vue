<template>
  <main>
    <!-- <div class="hammer">
      <div class="face"></div>
      <div class="head"></div>
      <div class="handle"></div>
    </div> -->
    <nav>
      <span>Whac-A-Mole</span>
      <span>High Score {{ cState.highScore }}</span>
    </nav>

    <div class="score">
      <h1>Score {{ cState.score }}</h1>
    </div>

    <div class="container">
      <div class="first-row">
        <div v-for="(mole, i) in cState.moles.slice(0, 2)" :key="i" class="whac col">
          <img
            @click="hit"
            src="@/assets/mole.svg"
            alt="mole"
            class="mole"
            :style="{
              opacity: mole.active ? 1 : 0,
              transform: mole.active ? 'translateY(0)' : 'translateY(100px)',
              transition: mole.active ? 'transform 0.3s ease' : 'none',
              cursor: mole.active ? 'pointer' : 'default',
              top: mole.active ? '1rem' : '5.5rem'
            }"
            v-if="cState.time < 20"
          />
          <img src="@/assets/hole.svg" alt="eclipse" class="eclipse" />
        </div>
      </div>

      <div class="second-row">
        <div v-for="(mole, i) in cState.moles.slice(2, 3)" :key="i" class="whac col">
          <img
            @click="hit"
            src="@/assets/mole.svg"
            alt="mole"
            class="mole"
            :style="{
              opacity: mole.active ? 1 : 0,
              transform: mole.active ? 'translateY(0)' : 'translateY(100px)',
              transition: mole.active ? 'transform 0.3s ease' : 'none',
              cursor: mole.active ? 'pointer' : 'default',
              top: mole.active ? '1rem' : '5.5rem'
            }"
            v-if="cState.time < 20"
          />
          <img src="@/assets/hole.svg" alt="eclipse" class="eclipse" />
        </div>
      </div>

      <div class="third-row">
        <div v-for="(mole, i) in cState.moles.slice(3, 5)" :key="i" class="whac col">
          <img
            @click="hit"
            src="@/assets/mole.svg"
            alt="mole"
            class="mole"
            :style="{
              opacity: mole.active ? 1 : 0,
              transform: mole.active ? 'translateY(0)' : 'translateY(100px)',
              transition: mole.active ? 'transform 0.3s ease' : 'none',
              cursor: mole.active ? 'pointer' : 'default',
              top: mole.active ? '1rem' : '5.5rem'
            }"
            v-if="cState.time < 20"
          />
          <img src="@/assets/hole.svg" alt="eclipse" class="eclipse" />
        </div>
      </div>
    </div>

    <div class="time">
      <div>
        <div class="slide">
          <div
            class="progress"
            :style="{
              width: cState.time * 5 + '%',
              backgroundColor: cState.time < 20 ? '#1D3208' : '#fbeee0'
            }"
          ></div>
        </div>
        <div class="timer-btw">
          <span>0s</span>
          <span>18s</span>
        </div>
      </div>
    </div>

    <div class="modal-overlay" v-if="cState.modal">
      <div class="modal">
        <div class="btw">
          <a href="/"><img src="@/assets/house.svg" alt="house" /></a>
          <span>High Score: {{ cState.highScore }}</span>
        </div>
        <h1>Your Score : {{ cState.score }}</h1>
        <button
          @click="
            () => {
              cState.score = 0
              startGame()
            }
          "
          class="button-74"
          role="button"
        >
          Play Again
        </button>
      </div>
    </div>
    <canvas
      ref="confettiCanvas"
      style="position: absolute; top: 0; left: 0; pointer-events: none"
    ></canvas>
  </main>
</template>

<style scoped>
main {
  min-height: 100vh;
  background-color: #afe57f;
  padding: 50px;
  font-family: 'Jomhuria', serif;
}

nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 4rem;
}

.score {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin: 2rem auto;
  font-size: 3rem;
  margin-bottom: 0;
}

.container {
  width: 40%;
  margin: auto;
  margin-top: 5rem;
}

.first-row {
  display: flex;
  justify-content: space-between;
}

.second-row {
  display: flex;
  justify-content: center;
  margin: 4rem auto;
}

.third-row {
  display: flex;
  justify-content: space-between;
}

.col {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: flex-end;
  width: 100px;
  height: 100px;
  position: relative;
  overflow: hidden;
}

.eclipse {
  width: 100%;
}

.mole {
  width: 80px;
  position: absolute;
  top: 5.5rem;
  right: 0.5rem;
  transition: opacity 0.3s ease-in-out;
  /* cursor: pointer; */
  opacity: 0;
}

.time {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin: 4rem auto;
  font-size: 2.5rem;
}

.hammer {
  height: 10rem;
  width: 10rem;
  position: fixed;
  left: 15%;
  top: 1%;
  /* Transitions with browser fallbacks */
  -webkit-transition: -webkit-transform 0.8s ease-in-out;
  -ms-transition: -ms-transform 0.8s ease-in-out;
  transition: transform 0.8s ease-in-out;
  z-index: 9999999;
  transform: rotate(-45deg);
  -ms-transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}

.face {
  height: 1rem;
  width: 1rem;
  background: #676767;
  position: absolute;
  left: 2.5rem;
  top: 0.25rem;
  border-radius: 2px;
}

.head {
  height: 2rem;
  width: 1.5rem;
  background: #676767;
  margin: 0 auto;
}

.head::before {
  content: '';
  height: 0;
  width: 0.5rem;
  position: absolute;
  left: 3rem;
  top: 0.25rem;
  border-bottom: 1rem solid #676767;
  border-left: 0.5rem solid transparent;
  border-right: 0.5rem solid transparent;
  transform: rotate(270deg);
}

.head::after {
  content: '';
  height: 1rem;
  width: 2rem;
  box-shadow: 6px 0.5rem 0 7px #676767;
  border-radius: 50%;
  position: absolute;
  right: 3rem;
  transform: rotate(180deg);
  top: 1rem;
}

.handle {
  height: 8rem;
  width: 1.5rem;
  margin: 0 auto;
  border-radius: 0 0 1rem 1rem;
  background: #ba6442;
  /* Textured wood effect */
  background-image: linear-gradient(90deg, rgba(255, 255, 255, 0.07) 50%, transparent 50%),
    linear-gradient(90deg, rgba(255, 255, 255, 0.13) 50%, transparent 50%),
    linear-gradient(90deg, transparent 50%, rgba(255, 255, 255, 0.17) 50%),
    linear-gradient(90deg, transparent 50%, rgba(255, 255, 255, 0.19) 50%);
  background-size: 13px, 29px, 37px, 53px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 99999;
}

.modal {
  position: absolute;
  top: 37%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #afe57f;
  width: 50%;
  border-radius: 50px;
  z-index: 999999;
  max-width: 600px;
  border: 20px solid rgba(2, 45, 17, 0.06);
  padding: 50px 25px;
  color: #022d11;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.modal h1 {
  text-align: center;
  font-size: 5rem;
  margin-top: 2rem;
}

.modal .house {
  width: 50px;
  cursor: pointer;
  background-color: #022d11;
}

.modal img {
  width: 50px;
  cursor: pointer;
}

.modal span {
  font-size: 2rem;
}

.btw {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 95%;
}

.button-74 {
  background-color: #fbeee0;
  border: 2px solid #422800;
  border-radius: 30px;
  box-shadow: #422800 4px 4px 0 0;
  color: #422800;
  cursor: pointer;
  display: inline-block;
  font-weight: 600;
  font-size: 18px;
  padding: 0 18px;
  line-height: 50px;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  margin: 2rem auto;
}
.button-74:hover {
  background-color: #fff;
}
.button-74:active {
  box-shadow: #422800 2px 2px 0 0;
  transform: translate(2px, 2px);
}

.slide {
  height: 20px;
  width: 30%;
  background-color: #00000034;
  border-radius: 10px;
  transition: width 0.3s ease-in-out;
  margin-top: 1rem;
  min-width: 300px;
  box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
}

.progress {
  height: 100%;
  background-color: #000000;
  border-radius: 10px;
}

.timer-btw {
  display: flex;
  justify-content: space-between;
  margin-top: 0.5rem;
}

@media (min-width: 768px) {
  .button-74 {
    min-width: 120px;
    padding: 0 25px;
  }
}

@media (max-width: 780px) {
  main {
    width: 100%;
    margin: 0;
    padding: 35px;
  }
  nav {
    font-size: 2.5rem;
    flex-wrap: wrap;
    width: 100%;
  }
  .container {
    width: 100%;
    margin: 0 auto;
  }

  .score {
    font-size: 2rem;
  }

  .modal {
    width: 90%;
    top: 50%;
  }

  .second-row {
    margin: 2rem auto;
  }

  .col {
    width: 80px;
    height: 80px;
  }

  .mole {
    width: 60px;
    top: 4.5rem;
  }
}
</style>

<script setup lang="ts">
import { onMounted, reactive, ref, watchEffect } from 'vue'
import confetti from 'canvas-confetti'

const confettiCanvas = ref<HTMLCanvasElement | null>(null)

const cState = reactive({
  activeMole: 0,
  score: 0,
  time: 0,
  timer: 0,
  modal: false,
  moles: [
    {
      id: 0,
      active: false
    },
    {
      id: 1,
      active: false
    },
    {
      id: 2,
      active: false
    },
    {
      id: 3,
      active: false
    },
    {
      id: 4,
      active: true
    }
  ],
  highScore: JSON.parse(localStorage.getItem('whac-a-highScore') || '0') || 0,
  isGameOver: false,
  isGameStarted: false,
  isGamePaused: false
})

const shuffleMoles = () => {
  cState.moles.forEach((mole) => {
    mole.active = false
  })
  const randomMole = Math.floor(Math.random() * cState.moles.length)
  cState.moles[randomMole].active = true
}

const hit = () => {
  cState.score++
  if (cState.time < 20) {
    shuffleMoles()
  }
}

const startGame = () => {
  cState.time = 0
  cState.modal = false
  cState.timer = setInterval(() => {
    cState.time++
    if (cState.time < 25) {
      shuffleMoles()
    }
  }, 900)
}

const celebrate = () => {
  confetti({
    particleCount: 100,
    spread: 70,
    origin: { y: 0.6 }
  })
}

const stopGame = () => {
  clearInterval(cState.timer)
  cState.modal = true
  if (cState.score > cState.highScore) {
    celebrate()
    cState.highScore = cState.score
    localStorage.setItem('whac-a-highScore', cState.highScore.toString())
  }
}

watchEffect(() => {
  if (cState.time === 20) {
    stopGame()
  }
})

startGame()

function cursor(e: any) {
  let mouseCursor: any = document.querySelector('.hammer')
  mouseCursor.style.top = e.pageY + 'px'
  mouseCursor.style.left = e.pageX + 'px'
  console.log(e.pageX, e.pageY)
}

onMounted(() => {
  // Set the canvas size to cover the entire viewport
  const { width, height } = document.body.getBoundingClientRect()
  if (confettiCanvas.value) {
    confettiCanvas.value.width = width
    confettiCanvas.value.height = height
  }
})

// window.addEventListener('mousemove', cursor)
</script>
