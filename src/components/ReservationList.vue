<script setup>
import BaseButton from './BaseButton.vue'

defineProps({
  game: {
    type: Object,
    required: true
  }
})

defineEmits(['reserve'])
</script>

<template>
  <div class="game-card">
    <h3>{{ game.name }}</h3>
    <p>👥 人數：{{ game.minPlayers }} - {{ game.maxPlayers }} 人</p>
    <p>
      狀態：<span :class="game.status">{{ game.status === 'available' ? '可預約' : '已預約' }}</span>
    </p>

    <!-- 使用共用按鈕元件，傳遞禁用狀態與點擊事件 -->
    <BaseButton 
      :disabled="game.status === 'reserved'" 
      @click="$emit('reserve')"
    >
      {{ game.status === 'available' ? '立即預約' : '已被預約（額滿）' }}
    </BaseButton>
  </div>
</template>

<style scoped>
.game-card { 
  background: white; 
  border: 1px solid #ddd; 
  padding: 15px; 
  border-radius: 8px; 
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.available { color: green; font-weight: bold; }
.reserved { color: red; font-weight: bold; }
</style>