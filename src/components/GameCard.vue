<template>
    <div class="card relative h-32 w-24" :turned=turned>
        <img class="card__front h-full w-full" :src=img alt="animal image">
        <img class="card__back h-full w-full cursor-pointer" src="../assets/backCard.jpg" alt="back of card">
    </div>
</template>
<script setup>
import { defineProps } from 'vue';

defineProps({
    img: String,
    turned: Boolean,
})
</script>

<style>
.card[turned=true] .card__front,.card[turned=true] .card__back{
    transform: rotateY(180deg);
}
.card__back{
    backface-visibility:hidden;
}
.card__front,.card__back{
    @apply rounded-md; 
    @apply border-2; 
    @apply border-black;
    @apply absolute;
    @apply object-cover;
    transition: all 0.45s;
    transform-style: preserve-3d;
}
</style>