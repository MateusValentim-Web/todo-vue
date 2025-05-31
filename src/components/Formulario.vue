<script setup>
const props = defineProps([
  'cadastraItem',
  'itemTemp',
  'precoTemp',
  'trocarFiltro',
  'editaItemTemp',
  'editaPrecoTemp'
])
</script>

<template>
  <form @submit.prevent="props.cadastraItem" class="mb-3">
    <div class="row g-2">
      <div class="col">
        <input 
          :value="props.itemTemp" 
          @input="props.editaItemTemp" 
          required 
          type="text" 
          placeholder="Item" 
          class="form-control"
        >
      </div>
      <div class="col">
        <input 
          :value="props.precoTemp" 
          @input="props.editaPrecoTemp" 
          required 
          type="number" 
          placeholder="Preço" 
          step="0.01"
          class="form-control"
        >
      </div>
      <div class="col-auto">
        <button type="submit" class="btn btn-primary">Adicionar</button>
      </div>
      <div class="col-md-3">
        <select @change="props.trocarFiltro" class="form-select">
          <option value="todas">Todas</option>
          <option value="pendentes">Pendentes</option>
          <option value="comprados">Comprados</option>
        </select>
      </div>
    </div>
  </form>
</template>
