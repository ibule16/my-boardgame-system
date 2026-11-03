<script setup>
defineProps({
  reservations: {
    type: Array,
    required: true
  }
})

defineEmits(['cancel'])
</script>

<template>
  <div class="reservation-list">
    <h2>📋 目前預約紀錄</h2>
    <div v-if="reservations.length === 0" class="empty">目前沒有預約紀錄</div>
    <ul>
      <li v-for="item in reservations" :key="item.id" class="reservation-item">
        <div>
          <strong>{{ item.gameName }}</strong> - 預約人：{{ item.userName || '訪客' }}
          <span class="time">（時間：{{ item.time || '未指定' }}）</span>
        </div>
        <button @click="$emit('cancel', item.id, item.gameId)" class="cancel-btn">
          取消預約
        </button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.reservation-list {
  background: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #ddd;
  margin-top: 20px;
}
.reservation-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: white;
  padding: 10px 15px;
  margin-bottom: 10px;
  border-radius: 6px;
  border: 1px solid #eee;
}
.cancel-btn {
  background-color: #ff4d4d;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
}
.cancel-btn:hover {
  background-color: #cc0000;
}
.empty {
  color: #888;
  text-align: center;
  padding: 10px;
}
</style>
