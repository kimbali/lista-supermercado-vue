<template>
  <div class="app-container">
    <h1>Lista de la compra</h1>

    <ul>
      <li v-for="(item, index) in items" :key="index">
        <div class="input-label">
          <!-- Wrap the checkbox and label in a div -->
          <input
            type="checkbox"
            :id="'item-' + index"
            v-model="item.checked"
            @change="updateSessionStorage"
          />
          <!-- Clicking on the label will check the checkbox -->
          <label
            class="label"
            :for="'item-' + index"
            :style="{ textDecoration: item.checked ? 'line-through' : 'none' }"
          >
            {{ item.name }}
          </label>
        </div>

        <button @click="removeItem(index)" class="trash">
          <i class="fas fa-trash"></i>
        </button>
      </li>
    </ul>

    <textarea v-model="newItems" placeholder="Añadir lista de alimentos"></textarea>
    <button class="add" @click="addItems">Añadir</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItems: '', // Aquí se almacenarán los nuevos elementos en formato de texto
      items: JSON.parse(localStorage.getItem('shoppingList')) || [] // Recupera la lista del sessionStorage
    }
  },
  methods: {
    // Función para agregar múltiples elementos
    addItems() {
      const itemList = this.newItems
        .split('\n') // Divide el texto en líneas
        .map((item) => item.trim()) // Elimina espacios en blanco
        .filter((item) => item) // Filtra las líneas vacías

      // Añade los nuevos elementos a la lista de items
      itemList.forEach((item) => {
        this.items.push({ name: item, checked: false })
      })

      this.updateSessionStorage() // Actualiza sessionStorage
      this.newItems = '' // Limpia el textarea
    },
    // Función para eliminar un elemento por su índice
    removeItem(index) {
      this.items.splice(index, 1)
      this.updateSessionStorage() // Actualiza sessionStorage
    },
    // Actualiza el sessionStorage cada vez que se agrega o elimina un elemento
    updateSessionStorage() {
      localStorage.setItem('shoppingList', JSON.stringify(this.items))
    }
  }
}
</script>

<style scoped>
#app {
}

.add {
  background-color: #7d80da;
  padding: 12px 8px;
  border-radius: 16px;
  text-transform: uppercase;
  color: white;
}

.app-container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  max-width: 390px;
  margin: 42px 24px 64px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  row-gap: 24px;
  color: #8e94f2;
}

.input-label {
  width: 100%;
  display: flex;
}

.label {
  width: 100%;
  display: flex;
  cursor: pointer;
}

textarea {
  height: 96px;
  border-radius: 16px;
  padding: 16px;
}

input[type='checkbox'] {
  margin-right: 10px;
}

button {
  margin-left: 10px;
  border: none;
  background-color: transparent;
}

button.trash svg {
  color: #b8b8ff;
  font-size: 18px;
}

h1,
ul,
li,
button {
  margin: 0;
  padding: 0;
}

li {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
  border-radius: 16px;
  padding-right: 12px;
}

li:hover {
  background-color: #7d80da;
  color: white;
}

body {
  margin: 24px;
}
</style>
