<script setup>
const props = defineProps({
  itens: Array,
  removeItem: Function,
  incrementarQuantidade: Function,
  decrementarQuantidade: Function,
  atualizarQuantidade: Function
});
</script>

<template>
  <ul class="list-group">
    <li v-for="(item, index) in itens" :key="item.id" class="list-group-item d-flex justify-content-between align-items-center">
      <div class="d-flex align-items-center">
        <input 
          type="checkbox" 
          v-model="item.comprado" 
          class="form-check-input me-2"
        />
        <span :class="{ 'text-decoration-line-through': item.comprado }">
          {{ item.nome }} - R$ {{ item.preco.toFixed(2) }}
        </span>
        
        <div class="quantity-controls ms-3">
          <button @click="() => props.decrementarQuantidade(item.id)" class="btn btn-sm btn-secondary">-</button>
          <input 
            type="number" 
            :value="item.quantidade" 
            @input="event => props.atualizarQuantidade(item.id, Number(event.target.value))"
            class="quantity-input" 
            min="1"
          />
          <button @click="() => props.incrementarQuantidade(item.id)" class="btn btn-sm btn-secondary">+</button>
        </div>
      </div>

      <button @click="() => removeItem(index)" class="btn btn-sm btn-danger ms-2">🗑️</button>
    </li>
  </ul>
</template>

<style scoped>
.text-decoration-line-through {
  text-decoration: line-through;
}

.quantity-controls {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  margin-left: 10px;
}

.quantity-input {
  width: 40px;
  text-align: center;
  padding: 2px;
}

/* Remover setinhas padrão do input number (Chrome, Edge, Safari) */
.quantity-input::-webkit-inner-spin-button,
.quantity-input::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Remover setinhas padrão no Firefox */
.quantity-input {
  -moz-appearance: textfield;
}
</style>
