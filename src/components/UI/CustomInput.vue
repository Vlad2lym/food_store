<script setup lang="ts">
    import { ref, nextTick } from 'vue'
    import CustomButton from './CustomButton.vue'
    import IconSearch from '../icons/IconSearch.vue'
    
    const isExpanded = ref(false)
    const searchQuery = ref('')
    const input = ref<HTMLInputElement | null>(null)
    
    const expandSearch = async () => {
      isExpanded.value = true
      await nextTick()
      input.value?.focus()
    }
    
    const collapseIfEmpty = () => {
      if (searchQuery.value === '') {
        isExpanded.value = false
      }
    }
</script>

<template>
  <div class="search-container" :class="{ expanded: isExpanded }">
    <CustomButton
      v-if="!isExpanded"
      class="search-button"
      @click="expandSearch"
    >
      <IconSearch/>
    </CustomButton>
    
    <div v-else class="search-input-wrapper">
      <input
        ref="input"
        v-model="searchQuery"
        class="search-input"
        type="text"
        placeholder="Поиск..."
        @blur="collapseIfEmpty"
      />
      <IconSearch class="search-icon-right" />
    </div>
  </div>
</template>


<style scoped>
.search-container {
  --size: 40px;
  position: relative;
  height: var(--size);
  transition: width 0.3s ease;
  width: var(--size);
}

.search-container.expanded {
  width: 250px;
}

.search-button {
  width: var(--size);
  height: var(--size);
  border: none;
  border-radius: 8px;
  background-color: #f9f9f9;
  color: #0f0f0f;
  cursor: pointer;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.5s ease;
}

.search-button:hover {
  background-color: #d0d0d0;
}

.search-input-wrapper {
  position: relative;
  height: 100%;
  width: 100%;
}

.search-input {
  width: 100%;
  height: 100%;
  border: 1px solid #282828;
  border-radius: 8px;
  padding: 0 40px 0 15px;
  font-size: 16px;
  outline: none;
  box-sizing: border-box;
  transition: all 0.5s ease;
}

.search-icon {
  width: 20px;
  height: 20px;
}

.search-icon-right {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  cursor: pointer;
}
</style>