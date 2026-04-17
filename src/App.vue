<script setup>
import { ref, computed } from 'vue'

const searchQuery = ref('')
const monsters = ref([
  { id: 1, name: 'Goblin', class: 'Guerreiro', level: 2 },
  { id: 2, name: 'Slime', class: 'Mago', level: 1 },
  { id: 3, name: 'Dragão', class: 'Chefe', level: 50 },
])

const filteredMonsters = computed(() => {
  return monsters.value.filter(monster =>
    monster.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>

<template>
  <div class="container mx-auto p-4 max-w-4xl">
    <h1 class="text-4xl font-bold mb-8 text-center text-red-500">RPG Bestiary</h1>
    
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
        class="bg-gray-800 p-5 rounded-lg shadow-lg border border-gray-700"
      >
        <h2 class="text-xl font-bold text-red-400">{{ monster.name }}</h2>
        <p class="text-gray-300">Classe: <span class="font-semibold">{{ monster.class }}</span></p>
        <p class="text-gray-300">Nível: <span class="font-semibold">{{ monster.level }}</span></p>
      </div>
    </div>
  </div>
</template>
