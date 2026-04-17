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
  <div class="container mx-auto p-4 max-w-4xl font-sans">
    <h1 class="text-5xl font-extrabold mb-8 text-center text-transparent bg-clip-text bg-gradient-to-r from-red-500 to-yellow-500 drop-shadow-lg">
      RPG Bestiary
    </h1>
    
    <div class="bg-gray-800 p-6 rounded-xl shadow-2xl border border-gray-700 mb-8 transition-transform hover:scale-[1.01]">
      <h2 class="text-2xl font-bold text-white mb-4">Adicionar Monstro</h2>
      <form @submit.prevent="addMonster" class="flex flex-col sm:flex-row gap-4">
        <input
          v-model="newMonster.name"
          type="text"
          placeholder="Nome"
          class="flex-1 p-3 rounded bg-gray-900 text-white border border-gray-600 focus:outline-none focus:ring-2 focus:ring-red-500 transition-shadow"
          required
        />
        <input
          v-model="newMonster.class"
          type="text"
          placeholder="Classe"
          class="flex-1 p-3 rounded bg-gray-900 text-white border border-gray-600 focus:outline-none focus:ring-2 focus:ring-red-500 transition-shadow"
          required
        />
        <input
          v-model.number="newMonster.level"
          type="number"
          min="1"
          placeholder="Nível"
          class="w-full sm:w-24 p-3 rounded bg-gray-900 text-white border border-gray-600 focus:outline-none focus:ring-2 focus:ring-red-500 transition-shadow"
          required
        />
        <button
          type="submit"
          class="bg-gradient-to-r from-red-600 to-red-800 hover:from-red-500 hover:to-red-700 text-white font-bold py-3 px-6 rounded shadow-lg transform transition hover:-translate-y-1"
        >
          Adicionar
        </button>
      </form>
    </div>

    <div class="mb-8">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Buscar monstro pelo nome..."
        class="w-full p-4 rounded-xl bg-gray-800 text-white border border-gray-700 shadow-inner focus:outline-none focus:ring-2 focus:ring-red-500 transition-shadow text-lg"
      />
    </div>

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
      <div
        v-for="monster in filteredMonsters"
        :key="monster.id"
        class="bg-gray-800 p-6 rounded-xl shadow-xl border border-gray-700 flex flex-col transform transition-all hover:-translate-y-2 hover:shadow-2xl hover:border-red-900"
      >
        <div v-if="editingId === monster.id" class="flex-1 space-y-3">
          <input v-model="editForm.name" class="w-full p-2 rounded bg-gray-900 text-white border border-gray-600 focus:ring-2 focus:ring-blue-500" />
          <input v-model="editForm.class" class="w-full p-2 rounded bg-gray-900 text-white border border-gray-600 focus:ring-2 focus:ring-blue-500" />
          <input v-model.number="editForm.level" type="number" class="w-full p-2 rounded bg-gray-900 text-white border border-gray-600 focus:ring-2 focus:ring-blue-500" />
          <div class="mt-4 flex gap-2">
            <button @click="saveEdit" class="flex-1 bg-green-600 hover:bg-green-500 text-white font-bold px-3 py-2 rounded shadow transition-colors">Salvar</button>
            <button @click="cancelEdit" class="flex-1 bg-gray-600 hover:bg-gray-500 text-white font-bold px-3 py-2 rounded shadow transition-colors">Cancelar</button>
          </div>
        </div>
        <template v-else>
          <div class="flex-1">
            <h2 class="text-2xl font-extrabold text-red-400 mb-2">{{ monster.name }}</h2>
            <div class="space-y-1">
              <p class="text-gray-400">Classe: <span class="font-bold text-gray-200">{{ monster.class }}</span></p>
              <p class="text-gray-400">Nível: <span class="font-bold text-yellow-500">{{ monster.level }}</span></p>
            </div>
          </div>
          <div class="mt-6 flex justify-end gap-3 border-t border-gray-700 pt-4">
            <button
              @click="startEdit(monster)"
              class="bg-blue-600 hover:bg-blue-500 text-white font-semibold py-1.5 px-4 rounded shadow transition-colors"
            >
              Editar
            </button>
            <button
              @click="deleteMonster(monster.id)"
              class="bg-red-800 hover:bg-red-600 text-white font-semibold py-1.5 px-4 rounded shadow transition-colors"
            >
              Deletar
            </button>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>
