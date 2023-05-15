<template>
  <StartMenu @setName="setPlayerName" v-if="showModal == 1" />
  <GameBoard :name=playerName :key=restartCounter @gameOver="gameOver" />
  <EndMenu @restart="restart" :misses="totalMisses" :name="playerName" v-if="showModal == 2" />
</template>

<script>
import { ref,defineComponent } from 'vue'
import GameBoard from './components/GameBoard.vue'
import StartMenu from './components/StartMenu.vue'
import EndMenu from './components/EndMenu.vue'

export default defineComponent({
  name: 'App',
  components: {
    GameBoard,
    StartMenu,
    EndMenu,
  },
  setup(){
    const playerName = ref('')
    const showModal = ref(1)
    const totalMisses = ref(0)
    const restartCounter = ref(0)
    
    const setPlayerName = (name) => { 
      playerName.value = name
      showModal.value = 0
    }

    const gameOver = (misses) => {
      showModal.value = 2
      totalMisses.value = misses
    }

    const restart = (id) => {
      restartCounter.value++
      if(id == 0){
        showModal.value = 1
        playerName.value = ''
      }
      else
        showModal.value = 0
    }

    return { 
      playerName,
      totalMisses,
      showModal,
      restartCounter,
      setPlayerName,
      gameOver,
      restart,
    }
  }
})
</script>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.modal{
  @apply h-screen;
  @apply w-screen;
  @apply absolute;
  @apply bg-black;
  @apply bg-opacity-70;
  @apply flex;
  @apply justify-center;
  @apply items-center;
  @apply z-10;
  @apply top-0;
}

.modal__container{
  @apply border-2;
  @apply border-slate-400;
  @apply bg-black;
  @apply rounded-lg;
  @apply px-20;
  @apply py-10;
  @apply flex;
  @apply flex-col;
  @apply space-y-8;
}

button{
  @apply rounded-lg;
  @apply border-2;
  @apply text-white;
  @apply p-2;
  @apply transition-all;
  @apply duration-300;
  @apply hover:bg-slate-200;
  @apply hover:text-slate-800;
}
button:hover{
  text-shadow: 0 0 .65px #333, 0 0 .65px #333;
}
</style>
