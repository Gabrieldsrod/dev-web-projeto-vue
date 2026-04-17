<script setup>
import { ref, computed } from 'vue'

const searchQuery = ref('')
const monsters = ref([
  { id: 1, name: 'Goblin', class: 'Guerreiro', level: 2 },
  { id: 2, name: 'Slime', class: 'Mago', level: 1 },
  { id: 3, name: 'Dragão', class: 'Chefe', level: 50 },
])

const newMonster = ref({ name: '', class: '', level: 1 })
const editingId = ref(null)
const editForm = ref({ name: '', class: '', level: 1 })

const addMonster = () => {
  if (!newMonster.value.name || !newMonster.value.class) return
  monsters.value.push({
    id: Date.now(),
    name: newMonster.value.name,
    class: newMonster.value.class,
    level: newMonster.value.level
  })
  newMonster.value = { name: '', class: '', level: 1 }
}

const deleteMonster = (id) => {
  monsters.value = monsters.value.filter(monster => monster.id !== id)
}

const startEdit = (monster) => {
  editingId.value = monster.id
  editForm.value = { ...monster }
}

const saveEdit = () => {
  const index = monsters.value.findIndex(m => m.id === editingId.value)
  if (index !== -1) {
    monsters.value[index] = { ...editForm.value, id: editingId.value }
  }
  editingId.value = null
}

const cancelEdit = () => {
  editingId.value = null
}

const filteredMonsters = computed(() => {
  return monsters.value.filter(monster =>
    monster.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>

<template>
  <div class="container mx-auto p-4 max-w-4xl">
    <h1 class="text-4xl font-bold mb-8 text-center text-red-500">RPG Bestiary</h1>
    
    <div class="bg-gray-800 p-5 rounded-lg shadow-lg border border-gray-700 mb-6">
      <h2 class="text-2xl font-bold text-white mb-4">Adicionar Monstro</h2>
      <form @submit.prevent="addMonster" class="flex flex-col sm:flex-row gap-4">
        <input
          v-model="newMonster.name"
          type="text"
          placeholder="Nome"
          class="flex-1 p-2 rounded bg-gray-700 text-white border border-gray-600 focus:outline-none focus:border-red-500"
          required
        />
        <input
          v-model="newMonster.class"
          type="text"
          placeholder="Classe"
          class="flex-1 p-2 rounded bg-gray-700 text-white border border-gray-600 focus:outline-none focus:border-red-500"
          required
        />
        <input
          v-model.number="newMonster.level"
          type="number"
          min="1"
          placeholder="Nível"
          class="w-full sm:w-24 p-2 rounded bg-gray-700 text-white border border-gray-600 focus:outline-none focus:border-red-500"
          required
        />
        <button
          type="submit"
          class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-6 rounded transition-colors"
        >
          Adicionar
        </button>
      </form>
    </div>

    <div class="mb-6">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Buscar monstro..."
        class="w-full p-3 rounded bg-gray-800 text-white border border-gray-700 focus:outline-none focus:border-red-500"
      />
    </div>

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
      <div
        v-for="monster in filteredMonsters"
        :key="monster.id"
        class="bg-gray-800 p-5 rounded-lg shadow-lg border border-gray-700 flex flex-col"
      >
        <div v-if="editingId === monster.id" class="flex-1 space-y-2">
          <input v-model="editForm.name" class="w-full p-1 rounded bg-gray-700 text-white border border-gray-600" />
          <input v-model="editForm.class" class="w-full p-1 rounded bg-gray-700 text-white border border-gray-600" />
          <input v-model.number="editForm.level" type="number" class="w-full p-1 rounded bg-gray-700 text-white border border-gray-600" />
          <div class="mt-2 flex gap-2">
            <button @click="saveEdit" class="bg-green-600 hover:bg-green-700 text-white px-2 py-1 rounded text-sm">Salvar</button>
            <button @click="cancelEdit" class="bg-gray-600 hover:bg-gray-700 text-white px-2 py-1 rounded text-sm">Cancelar</button>
          </div>
        </div>
        <template v-else>
          <div class="flex-1">
            <h2 class="text-xl font-bold text-red-400">{{ monster.name }}</h2>
            <p class="text-gray-300">Classe: <span class="font-semibold">{{ monster.class }}</span></p>
            <p class="text-gray-300">Nível: <span class="font-semibold">{{ monster.level }}</span></p>
          </div>
          <div class="mt-4 flex justify-end gap-2">
            <button
              @click="startEdit(monster)"
              class="bg-blue-600 hover:bg-blue-700 text-white text-sm py-1 px-3 rounded transition-colors"
            >
              Editar
            </button>
            <button
              @click="deleteMonster(monster.id)"
              class="bg-red-900 hover:bg-red-800 text-red-200 text-sm py-1 px-3 rounded transition-colors"
            >
              Deletar
            </button>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>
