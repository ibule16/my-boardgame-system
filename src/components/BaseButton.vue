<script setup>
defineProps({
  disabled: {
    type: Boolean,
    default: false
  },
  variant: {
    type: String,
    default: 'primary' // 'primary' (主按鈕), 'danger' (危險/取消按鈕)
  }
})

defineEmits(['click'])
</script>

<template>
  <button 
    class="base-btn" 
    :class="[variant, { disabled: disabled }]"
    :disabled="disabled"
    @click="$emit('click')"
  >
    <!-- Vue 的 slot（插槽），讓按鈕裡面的文字可以由外部動態決定 -->
    <slot></slot>
  </button>
</template>

<style scoped>
.base-btn {
  padding: 8px 14px;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.2s, opacity 0.2s;
}

/* 主按鈕樣式（例如：立即預約） */
.base-btn.primary {
  background: #42b883;
  color: white;
}
.base-btn.primary:hover:not(.disabled) {
  background: #33a06f;
}

/* 危險按鈕樣式（例如：取消預約） */
.base-btn.danger {
  background: #ff4d4f;
  color: white;
}
.base-btn.danger:hover:not(.disabled) {
  background: #e04345;
}

/* 禁用狀態（例如：額滿/已被預約） */
.base-btn.disabled {
  background: #e0e0e0 !important;
  color: #888 !important;
  cursor: not-allowed;
  opacity: 0.7;
}
</style>