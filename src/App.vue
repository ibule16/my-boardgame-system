<script setup>
import { ref, computed, watch } from 'vue'
import GameCard from './components/GameCard.vue'
import GameFilterBar from './components/GameFilterBar.vue'
import ReservationList from './components/ReservationList.vue'
import ReserveModal from './components/ReserveModal.vue'

// 1. 狀態初始化
const boardgames = ref(JSON.parse(localStorage.getItem('boardgames') || JSON.stringify([
  { id: 1, name: '卡卡頌 (Carcassonne)', category: '策略益智', minPlayers: 2, maxPlayers: 5, playTime: 45, status: 'available' },
  { id: 2, name: '卡坦島 (Catan)', category: '策略交易', minPlayers: 3, maxPlayers: 4, playTime: 90, status: 'available' }
])))

const reservations = ref(JSON.parse(localStorage.getItem('reservations') || '[]'))

watch(boardgames, (val) => localStorage.setItem('boardgames', JSON.stringify(val)), { deep: true })
watch(reservations, (val) => localStorage.setItem('reservations', JSON.stringify(val)), { deep: true })

// 2. 篩選與 Modal 狀態
const searchQuery = ref('')
const selectedCategory = ref('all')
const showReserveModal = ref(false)
const selectedGame = ref(null)

const filteredGames = computed(() => {
  return boardgames.value.filter(game => {
    const matchesSearch = game.name.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchesCategory = selectedCategory.value === 'all' || game.category === selectedCategory.value
    return matchesSearch && matchesCategory
  })
})

// 3. 業務邏輯方法
const openReserveModal = (game) => {
  selectedGame.value = game
  showReserveModal.value = true
}

const handleReserveSubmit = (formData) => {
  if (!selectedGame.value) return
  selectedGame.value.status = 'reserved'
  
  reservations.value.unshift({
    id: Date.now(),
    gameId: selectedGame.value.id,
    gameName: selectedGame.value.name,
    ...formData
  })
  
  showReserveModal.value = false
}

const cancelReservation = (reservationId, gameId) => {
  const game = boardgames.value.find(g => g.id === gameId)
  if (game) game.status = 'available'
  reservations.value = reservations.value.filter(r => r.id !== reservationId)
}
</script>

<template>
  <div class="container">
    <h1>🎲 桌遊預約管理系統</h1>

    <!-- 搜尋與篩選元件 -->
    <GameFilterBar 
      v-model:searchQuery="searchQuery" 
      v-model:selectedCategory="selectedCategory" 
    />

    <!-- 桌遊列表網格 -->
    <div class="game-grid">
      <GameCard 
        v-for="game in filteredGames" 
        :key="game.id" 
        :game="game" 
        @reserve="openReserveModal(game)"
      />
    </div>

    <!-- 預約紀錄清單元件 -->
    <ReservationList 
      :reservations="reservations" 
      @cancel="cancelReservation" 
    />

    <!-- 預約彈窗元件 -->
    <ReserveModal 
      :show="showReserveModal" 
      :game="selectedGame" 
      @close="showReserveModal = false" 
      @submit="handleReserveSubmit" 
    />
  </div>
</template>

<style scoped>
.container { max-width: 800px; margin: 0 auto; padding: 20px; font-family: sans-serif; }
.game-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 15px; margin-bottom: 20px; }
</style>