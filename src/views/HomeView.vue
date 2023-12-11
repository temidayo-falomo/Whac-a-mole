<script setup lang="ts">
import { useRouter } from 'vue-router'
const router = useRouter()

const goToGame = () => {
  router.push('/game')
}
</script>

<template>
  <main>
    <div class="col">
      <h2>Whac-A-Mole</h2>
      <button @click="goToGame" class="button-74" role="button">Start Game</button>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #afe57f;
  padding: 0 2rem;
}

.col {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

h2 {
  font-size: 12rem;
  text-align: center;
}

/* CSS */
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
}
.button-74:hover {
  background-color: #fff;
}
.button-74:active {
  box-shadow: #422800 2px 2px 0 0;
  transform: translate(2px, 2px);
}
@media (min-width: 768px) {
  .button-74 {
    min-width: 120px;
    padding: 0 25px;
  }
}

@media (max-width: 650px) {
  h2 {
    font-size: 6rem;
  }
}
</style>
