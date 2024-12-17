<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import CustomButton from './components/CustomButton.vue'
import { getRandomValue } from './helpers/getRandomValue'

const currentRound = ref(0)
const playerHealth = ref(100)
const monsterHealth = ref(100)
const monsterImageAnimation = ref('')

const monsterBarStyles = computed(() => {
  if (monsterHealth.value <= 0) {
    return { width: '0%' }
  }

  return { width: `${monsterHealth.value}%` }
})

const playerBarStyles = computed(() => {
  if (playerHealth.value <= 0) {
    return { width: '0%' }
  }

  return { width: `${playerHealth.value}%` }
})

// watch(monsterHealth, (newValue, oldValue) => {
//   if (newValue < oldValue) {
//     monsterImageAnimation.value = 'animate-pulse'
//   }

//   if (newValue > oldValue) {
//     monsterImageAnimation.value = 'animate-bounce'
//   }

//   setTimeout(() => {
//     monsterImageAnimation.value = ''
//   }, 2000)
// })

const attackPlayer = () => {
  const attackValue = getRandomValue(8, 15)

  playerHealth.value -= attackValue

  monsterImageAnimation.value = 'animate-ping'

  setTimeout(() => {
    monsterImageAnimation.value = ''
  }, 3000)
}

const attackMonster = () => {
  currentRound.value++

  const attackValue = getRandomValue(5, 12)
  monsterHealth.value -= attackValue

  attackPlayer()
}

const specialAttackMonster = () => {
  currentRound.value++

  const attackValue = getRandomValue(10, 25)
  monsterHealth.value -= attackValue

  attackPlayer()
}

const healPlayer = () => {
  currentRound.value++

  const healValue = getRandomValue(8, 20)
  const newPlayerHealth = playerHealth.value + healValue

  if (newPlayerHealth > 100) {
    playerHealth.value = 100
  } else {
    playerHealth.value = newPlayerHealth
  }

  attackPlayer()
}
</script>

<template>
  <main
    class="bg-bg h-screen bg-[linear-gradient(to_right,#80808033_1px,transparent_1px),linear-gradient(to_bottom,#80808033_1px,transparent_1px)] bg-[size:70px_70px] flex flex-col justify-center items-center p-20"
  >
    <div class="max-w-[1200px] flex flex-col gap-8">
      <h1 class="text-3xl font-bold font-mono text-main text-center mb-4">Monster Smash</h1>

      <section class="flex w-full">
        <img src="./assets/monster.svg" alt="" class="w-24" :class="monsterImageAnimation" />
        <div class="w-full">
          <h2>Monster Health</h2>
          <div class="w-full h-10 border-s-gray-600 m-1 bg-purple-300">
            <div class="w-full h-full bg-purple-800" :style="monsterBarStyles"></div>
          </div>
        </div>
      </section>

      <section class="flex w-full">
        <img src="./assets/player.svg" alt="" class="w-24" />
        <div class="w-full">
          <h2>Your Health</h2>
          <div class="w-full h-10 border-s-gray-600 m-1 bg-purple-300">
            <div class="w-full h-full bg-purple-800" :style="playerBarStyles"></div>
          </div>
        </div>
      </section>

      <section class="flex gap-4">
        <custom-button :onClick="attackMonster">Attack</custom-button>
        <custom-button :onClick="specialAttackMonster">Special Attack</custom-button>
        <custom-button :onClick="healPlayer">Heal</custom-button>
        <custom-button>Surrender</custom-button>
      </section>
    </div>
  </main>
</template>
