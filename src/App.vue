<script setup>
// Importando la fabrica de referencias reactivas
import { ref } from 'vue';

// Creando la referencia reactiva
const header = ref("Lista de compras");
const editing = ref(false);
const items = ref([
    {id: 1, label:'10 Nintendo Switch'},
    {id: 2, label:'1 Xbox Series S'},
    {id: 3, label:'2 Polistation'},
]);
const newItem = ref('');
const newItemHighPriority = ref(false);

// Metodo para agregar un item
const saveItem = () => {
  items.value.push({id: items.value.length + 1, label: newItem.value});
}

// Metodo para entrar a modo edicion
const doEdit = (edit) => {
    editing.value = edit;
} 
</script>

<template>
<div class="header">
  <h1>🛒 {{ header }} </h1>
  <button v-if="editing" @click="doEdit(false)" class="btn">Cancelar</button>
  <button v-else @click="doEdit(true)" class="btn-primary">Agregar articulo</button>
</div>
  <div v-if="editing" class="add-item form">
    <input @keyup.enter="saveItem" v-model="newItem" type="text" placeholder="Add Item" />
    <label>
        <input type="checkbox" v-model="newItemHighPriority"/>
    </label>
    <button @click="saveItem" class="btn btn-primary">Guardar Articulo</button>
  </div>
  
  <ul>
    <li v-for="({id, label}, index) in items" :key="id">🔅 {{ label }}</li>
  </ul>
  <p v-if='items.length === 0'>🌹 No hay elementos en la lista</p>
</template>