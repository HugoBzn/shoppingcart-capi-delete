<script setup>
// Importando la fabrica de referencias reactivas
import { ref, computed } from "vue";

// Creando la referencia reactiva
const header = ref("Lista de compras");
const items = ref([
  { id: 1, label: "10 Nintendo Switch", purchased: false, highPriority: false },
  { id: 2, label: "1 Xbox Series S", purchased: true, highPriority: true },
  { id: 3, label: "2 Polistation", purchased: true, highPriority: false },
]);
const newItem = ref("");
const newItemHighPriority = ref(false);
const editing = ref(false);

/* Metodos */
// Metodo para agregar un item
const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    purchased: false,
    highPriority: newItemHighPriority.value,
  });
  clearForm();
};

// Metodo para limpiar cuando cancele
const cancel = () => {
  clearForm();
};

// Metodo para entrar a modo edicion
const doEdit = (edit) => {
  editing.value = edit;
};

// Metodo para los articulos comprados
const tooglePurchased = (item) => {
  item.purchased = !item.purchased;
};

// Metodo clearForm
const clearForm = () => {
  newItem.value = "";
  newItemHighPriority.value = false;
};

/* Propiedades computadas */
// Creando la propiedad computada
const characterCount = computed(() => {
  return newItem.value.length;
});

// Colocar la lista al revés
// Creando propiedad computada que invierte items de la lista
const reversedItems = computed(() => [...items.value].reverse());
</script>

<template>
  <!-- Ventana inicial -->
  <div class="header">
    <h1>🛒 {{ header }}</h1>
    <button
      v-if="editing"
      @click="doEdit(false), cancel()"
      class="btn btn-cancel"
    >
      Cancelar
    </button>
    <button v-else @click="doEdit(true)" class="btn btn-primary">
      Agregar articulo
    </button>
  </div>

  <!-- Entra al modo edicion -->
  <div v-if="editing" class="add-item-form">
    <input
      @keyup.enter="saveItem"
      v-model="newItem"
      type="text"
      placeholder="Add Item"
      maxlength="100"
    />
    <!-- Contador de caracteres disponibles -->
    <label>{{ characterCount }} / 100</label>
    <label>
      <input type="checkbox" v-model="newItemHighPriority" />
      <strong>Priority level</strong>
    </label>
    <button @click="saveItem" class="btn btn-primary">Guardar Articulo</button>
  </div>

  <!-- En caso de que no haya articulos que mostrar -->
  <p v-if="items.length === 0">🌹 No hay elementos en la lista</p>
  <!-- Renderizado de la lista -->
  <ul>
    <li
      v-for="({ id, label, purchased, highPriority }, index) in reversedItems"
      :key="id"
      @click="tooglePurchased(reversedItems[index])"
      :class="{
        strikeout: purchased,
        priority: highPriority,
      }"
    >
      {{ highPriority ? "🥵" : "😄" }} {{ label }}
    </li>
  </ul>
</template>
