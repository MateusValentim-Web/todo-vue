<script setup>
const props = defineProps(['totalComprado'])
</script>

<template>
  <header class="p-3 mb-4 bg-light rounded-3">
    <h1>Lista de Compras</h1>
    <p>Total de Compras: R$ {{ props.totalComprado.toFixed(2) }}</p>
  </header>
</template>
