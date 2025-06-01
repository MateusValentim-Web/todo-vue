<script setup>
import { reactive, computed, watch } from 'vue'
import Cabecalho from './components/Cabecalho.vue'
import Formulario from './components/Formulario.vue'
import ListaDeCompras from './components/ListaDeCompras.vue'

const estado = reactive({
  filtro: 'todas',
  itemTemp: '',
  precoTemp: '',
  itens: []
})

// Carregar itens salvos do localStorage ao iniciar
const itensSalvos = localStorage.getItem('lista-de-compras')
if (itensSalvos) {
  try {
    estado.itens = JSON.parse(itensSalvos)
  } catch (e) {
    console.error('Erro ao ler lista do localStorage:', e)
  }
}

const itensFiltrados = computed(() => {
  switch (estado.filtro) {
    case 'comprados':
      return estado.itens.filter(item => item.comprado)
    case 'pendentes':
      return estado.itens.filter(item => !item.comprado)
    default:
      return estado.itens
  }
})

const totalComprado = computed(() => {
  return estado.itens
    .filter(item => item.comprado)
    .reduce((soma, item) => soma + item.preco * (item.quantidade || 1), 0)
})

function cadastraItem() {
  if (!estado.itemTemp || !estado.precoTemp) return

  const novoItem = {
    id: Date.now(),
    nome: estado.itemTemp,
    preco: parseFloat(estado.precoTemp),
    comprado: false,
    quantidade: 1 // inicia com 1
  }
  estado.itens.push(novoItem)
  estado.itemTemp = ''
  estado.precoTemp = ''
}

function trocarFiltro(evento) {
  estado.filtro = evento.target.value
}

function editaItemTemp(evento) {
  estado.itemTemp = evento.target.value
}

function editaPrecoTemp(evento) {
  estado.precoTemp = evento.target.value
}

function removeItem(index) {
  estado.itens.splice(index, 1)
}

function incrementarQuantidade(itemId) {
  const item = estado.itens.find(i => i.id === itemId)
  if (item) item.quantidade = (item.quantidade || 1) + 1
}

function decrementarQuantidade(itemId) {
  const item = estado.itens.find(i => i.id === itemId)
  if (item && item.quantidade > 1) item.quantidade -= 1
}

// Salvar itens no localStorage sempre que mudarem
watch(() => estado.itens, (novoValor) => {
  localStorage.setItem('lista-de-compras', JSON.stringify(novoValor))
}, { deep: true })
</script>

<template>
  <div class="container mt-5">
    <Cabecalho :total-comprado="totalComprado" />
    <Formulario 
      :item-temp="estado.itemTemp" 
      :preco-temp="estado.precoTemp"
      :trocar-filtro="trocarFiltro"
      :edita-item-temp="editaItemTemp"
      :edita-preco-temp="editaPrecoTemp"
      :cadastra-item="cadastraItem"
    />
    <ListaDeCompras 
      :itens="itensFiltrados" 
      :remove-item="removeItem"
      :incrementar-quantidade="incrementarQuantidade"
      :decrementar-quantidade="decrementarQuantidade"
    />
  </div>
</template>
