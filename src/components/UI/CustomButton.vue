<script setup lang="ts">
const props = defineProps({
    active: {
    type: Boolean,
    required: false,
    default: false
  }
})
</script>

<template>
    <button 
        class="custom-button"
        :class={active}
        @click="$emit('click')"
    >
        <slot></slot>
    </button>
</template>

<style scoped>
.custom-button {
  position: relative;
  padding: 10px 10px;
  border: none;
  border-radius: 8px;
  background-color: #f9f9f9;
  color: #0f0f0f;
  cursor: pointer;
  transition: background-color 0.2s ease;
  font-size: 18px;
}

.custom-button:hover {
  background-color: #d0d0d0;
}

.custom-button.active {
  background-color: #282828;
  color: #f2f2f2;
}
</style>
