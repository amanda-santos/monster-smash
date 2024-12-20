<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import CustomButton from './components/CustomButton.vue'
import { getRandomValue } from './helpers/getRandomValue'

const currentRound = ref(0)
const playerHealth = ref(100)
const monsterHealth = ref(100)
const winner = ref<'player' | 'monster' | 'draw' | null>(null)
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

const canUseSpecialAttack = computed(() => currentRound.value % 3 !== 0)

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

const surrender = () => {
  winner.value = 'monster'
}

watch([playerHealth, monsterHealth], ([playerHealth, monsterHealth]) => {
  if (playerHealth <= 0 && monsterHealth <= 0) {
    winner.value = 'draw'
  } else if (playerHealth <= 0) {
    winner.value = 'monster'
  } else if (monsterHealth <= 0) {
    winner.value = 'player'
  }
})
</script>

<template>
  <main
    class="flex h-screen flex-col items-center justify-center bg-bg bg-[linear-gradient(to_right,#80808033_1px,transparent_1px),linear-gradient(to_bottom,#80808033_1px,transparent_1px)] bg-[size:20px_20px] p-20"
  >
    <div class="flex min-w-[400px] flex-col gap-8">
      <h1 class="mb-4 text-center font-mono text-5xl font-bold uppercase text-text">
        > Monster Smash
      </h1>

      <section class="flex w-full">
        <img src="./assets/monster.svg" alt="" class="w-24" />
        <div class="w-full">
          <h2 class="font-mono font-semibold uppercase text-text">> Monster</h2>
          <div class="m-1 h-10 w-full border-4 border-border bg-border">
            <div class="h-full w-full bg-main" :style="monsterBarStyles"></div>
          </div>
        </div>
      </section>

      <section class="flex w-full">
        <img src="./assets/player.svg" alt="" class="w-24" />
        <div class="w-full">
          <h2 class="font-mono font-semibold uppercase text-text">> You</h2>
          <div class="m-1 h-10 w-full border-4 border-border bg-border">
            <div class="h-full w-full bg-main" :style="playerBarStyles"></div>
          </div>
        </div>
      </section>

      <section
        v-if="winner"
        class="border-2 border-border bg-green-400 p-4 text-center font-mono text-3xl font-bold uppercase text-text"
        :class="{
          'bg-green-500': winner === 'player',
          'bg-red-500': winner === 'monster',
          'bg-blue-400': winner === 'draw',
        }"
      >
        <h2 v-if="winner === 'monster'">💀 You lost!</h2>
        <h2 v-else-if="winner === 'player'">🏆 You won!</h2>
        <h2 v-else-if="winner === 'draw'">It's a draw!</h2>
      </section>

      <section class="flex gap-4" v-else>
        <custom-button :onClick="attackMonster">Attack</custom-button>
        <custom-button :onClick="specialAttackMonster" :disabled="canUseSpecialAttack">
          Special Attack
        </custom-button>
        <custom-button :onClick="healPlayer">Heal</custom-button>
        <custom-button :onClick="surrender">Surrender</custom-button>
      </section>
    </div>
  </main>
</template>
