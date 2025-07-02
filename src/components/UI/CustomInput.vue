<script setup lang="ts">
    import { ref, nextTick } from 'vue'
    import CustomButton from '@/components/UI/CustomButton.vue'
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
      class="search-button"
      @click="expandSearch"
    >
      <IconSearch/>
    </CustomButton>
    
    <div class="search-input-wrapper">
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
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  left: 0;
  top: 0;
  transition: opacity 0.2s ease 0.1s, transform 0.3s ease 0.1s;}

.search-input-wrapper {
  height: 100%;
  width: 100%;
  position: absolute;
  left: 0;
  top: 0;
  transition: opacity 0.2s ease, transform 0.3s ease;
}

.search-input {
  width: 100%;
  height: 100%;
  border: 1px solid #d0d0d0;
  border-radius: 8px;
  padding: 0 40px 0 15px;
  font-size: 16px;
  outline: none;
  box-sizing: border-box;
  transition: all 0.5s ease;
}

.search-icon {
  width: 15px;
  height: 15px;
}

.search-icon-right {
  position: absolute;
  right: 13px;
  top: 50%;
  transform: translateY(-50%);
  width: 15px;
  height: 15px;
  cursor: pointer;
}

.search-container.expanded .search-button {
  opacity: 0;
  transform: translateX(10px);
  pointer-events: none;
}

.search-container:not(.expanded) .search-input-wrapper {
  opacity: 0;
  transform: translateX(10px);
  pointer-events: none;
}
</style>