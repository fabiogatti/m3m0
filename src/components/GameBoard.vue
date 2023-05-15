<template>
  <div class="board flex flex-col items-center justify-center min-h-screen space-y-8 bg-black">
    <p class="text-4xl pt-6">Jugador: <span class="text-slate-400">{{ name }}</span></p>
    <div class="justify-center w-full max-w-6xl grid grid-cols-2 xs:grid-cols-4 sm:grid-cols-6 lg:grid-cols-8 gap-x-[3%] gap-y-[1vh]">
      <GameCard 
        class="justify-self-center"
        v-for="item in animals" 
        :key="item.id" 
        :img="item.img" 
        :turned="item.flipped" 
        @click.prevent="clickedCard(item.id)"
      />
    </div>
    <div class="flex flex-row text-lg pb-6">
      <p class="mr-[10vh]">Aciertos: <span class="text-slate-400">{{ hit }}</span></p>
      <p>Errores: <span class="text-slate-400">{{ miss }}</span></p>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref, onMounted } from 'vue'
import GameCard from './GameCard.vue'

defineProps({
  name: String
})

const emit = defineEmits(['gameOver'])

const animals = ref([])
const hit = ref(0)
const miss = ref(0)
const turn = ref(0)
const lastClickId = ref(0)
const readyToClick = ref(true)

const shuffleArr = (arr,n) => {
  if(n==0)
    return arr
  return shuffleArr(arr.sort(() => 0.5 - Math.random()),n-1)
}

onMounted(async() => {
  const response = await fetch("https://fed-team.modyo.cloud/api/content/spaces/animals/types/game/entries?per_page=12")
  const fetchData = await response.json()
  let animalId = 0
  fetchData.entries.map(animal => {
    let newAnimal = { 
      id: animalId,
      name: animal.meta.name,
      img: animal.fields.image.url,
      flipped: false,
    }
    animals.value.push({ ...newAnimal })
    newAnimal.id = animalId + 1
    animals.value.push({ ...newAnimal })
    animalId += 2
  })
  shuffleArr(animals.value,10);
})

const turnCard = (id) => {
  animals.value.find((item) => item.id == id).flipped = true;
}

const clickedCard = async(id) => {
  if(id === lastClickId.value || readyToClick.value == false)
    return
  turnCard(id)
  if(turn.value == 1){
    readyToClick.value = false
    setTimeout(() => {
      const currentId = animals.value.findIndex((item) => item.id == id)
      const lastId = animals.value.findIndex((item) => item.id == lastClickId.value)
      if(animals.value[currentId].name != animals.value[lastId].name){
        animals.value[currentId].flipped = false
        animals.value[lastId].flipped = false
        miss.value++
      }
      else{
        hit.value++
        if(hit.value == 12){
          emit('gameOver',miss.value)
        }
      }
      turn.value = 0
      readyToClick.value = true
    }, 1000);
  }
  else{
    lastClickId.value = id
    turn.value++
  }
}

</script>