<script setup>
const props = defineProps({
  itens: Array,
  removeItem: Function
})
</script>

<template>
  <ul class="list-group">
    <li 
      v-for="(item, index) in props.itens" 
      :key="item.id" 
      class="list-group-item d-flex justify-content-between align-items-center"
    >
      <div>
        <input 
          type="checkbox" 
          v-model="item.comprado" 
          @change="evento => item.comprado = evento.target.checked"
          class="form-check-input me-2"
        />
        <span :class="{ 'text-decoration-line-through': item.comprado }">
          {{ item.nome }} - R$ {{ item.preco.toFixed(2) }}
        </span>
      </div>
      <button 
        @click="() => props.removeItem(index)" 
        class="btn btn-sm btn-danger"
      >
        🗑️
      </button>
    </li>
  </ul>
</template>

<style scoped>
.text-decoration-line-through {
  text-decoration: line-through;
}
</style>
