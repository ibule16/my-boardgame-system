<script setup>
defineProps({
  searchQuery: String,
  selectedCategory: String
})

defineEmits(['update:searchQuery', 'update:selectedCategory'])
</script>

<template>
  <div class="filter-card">
    <input 
      :value="searchQuery" 
      @input="$emit('update:searchQuery', $event.target.value)" 
      type="text" 
      placeholder="🔍 搜尋桌遊..." 
    />
    <select 
      :value="selectedCategory" 
      @change="$emit('update:selectedCategory', $event.target.value)"
    >
      <option value="all">全部分類</option>
      <option value="策略益智">策略益智</option>
      <option value="策略交易">策略交易</option>
    </select>
  </div>
</template>

<style scoped>
.filter-card { display: flex; gap: 10px; margin-bottom: 20px; }
input, select { padding: 8px; border: 1px solid #ddd; border-radius: 4px; flex: 1; }
</style>