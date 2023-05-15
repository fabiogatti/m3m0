<template>
     <div class="modal">
        <div class="modal__container">
            <h2 class="text-3xl text-slate-400">¡Felicitaciones <span class="text-slate-800">{{ name }}</span>!</h2>
            <p>Te tomo un total de <span class="text-slate-400">{{ misses }}</span> fallos</p>
            <p>¿Quieres intentarlo nuevamente?</p>
            <div class="flex flex-row justify-between space-x-6">
                <button @click="$emit('restart',0)">Ir al Inicio</button>
                <button @click="$emit('restart',1)">Jugar de nuevo</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

defineProps({
    name: String,
    misses: Number,
})

defineEmits(['restart'])
</script>