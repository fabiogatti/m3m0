<template>
    <div class="modal">
        <div class="modal__container">
            <h2 class="text-3xl text-slate-400">M<span class="text-white">3</span>M<span class="text-white">0</span></h2>
            <div class="flex flex-col space-y-3 pb-4">
                <label>Ingresa tu nombre</label>
                <input required v-model="name" class="outline-none rounded-lg px-2 py-1 bg-slate-500 text-white" >
            </div>
            <button @click="sendName">EMPEZAR</button>
        </div>
    </div>
</template>
<script setup>
import { ref,defineEmits } from 'vue'

const emit = defineEmits(['setName'])

const sendName = () => {
    if(name.value === '')
        alert('Favor ingresar tu nombre antes de comenzar')
    else
        emit('setName',name.value)
}

const name = ref('')
</script>