<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  show: Boolean,
  game: Object
})

const emit = defineEmits(['close', 'submit'])

const form = ref({
  name: '',
  phone: '',
  date: '',
  timeSlot: '下午場 (14:00 - 17:00)',
  peopleCount: 2
})

// 當選擇的遊戲改變時，自動將人數預設為該遊戲的最少人數
watch(() => props.game, (newGame) => {
  if (newGame) {
    form.value.peopleCount = newGame.minPlayers
  }
})

const handleSubmit = () => {
  emit('submit', { ...form.value })
  // 清空表單
  form.value.name = ''
  form.value.phone = ''
  form.value.date = ''
}
</script>

<template>
  <div v-if="show" class="modal-backdrop" @click.self="$emit('close')">
    <div class="modal">
      <h3>預約：{{ game?.name }}</h3>
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label>姓名：</label>
          <input v-model="form.name" type="text" required />
        </div>
        <div class="form-group">
          <label>電話：</label>
          <input v-model="form.phone" type="tel" required />
        </div>
        <div class="form-group">
          <label>日期：</label>
          <input v-model="form.date" type="date" required />
        </div>
        <div class="modal-actions">
          <button type="submit" class="btn-submit">確認預約</button>
          <button type="button" class="btn-cancel" @click="$emit('close')">取消</button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.modal-backdrop { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); display: flex; justify-content: center; align-items: center; }
.modal { background: white; padding: 20px; border-radius: 8px; width: 350px; }
.form-group { margin-bottom: 12px; }
.form-group label { display: block; margin-bottom: 4px; font-size: 14px; }
.form-group input { width: 100%; padding: 6px; box-sizing: border-box; border: 1px solid #ddd; border-radius: 4px; }
.modal-actions { display: flex; gap: 10px; justify-content: flex-end; margin-top: 15px; }
.btn-submit { background: #42b883; color: white; border: none; padding: 6px 12px; border-radius: 4px; cursor: pointer; }
.btn-cancel { background: #ccc; border: none; padding: 6px 12px; border-radius: 4px; cursor: pointer; }
</style>