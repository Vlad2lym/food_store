<script setup lang="ts">
    import { ref, onMounted, onBeforeUnmount } from 'vue'
    const props = defineProps({
        options: {
            type: Array<{
                id: number,
                title: string,
                value: any
            }>,
            required: true,
        }
    })

    const isOpen = ref(false)
    const select = ref<HTMLElement | null>(null)
    const currentItem = ref(props.options?.length ? props.options[0] : { id: 0, title: '', value: '' })

    const toggleDropdown = () => {
        isOpen.value = !isOpen.value
    }

    const selectItem = (item: typeof props.options[0]) => {
        currentItem.value = item
        isOpen.value = false
    }

    const handleClickOutside = (event: MouseEvent) => {
        if (select.value && !select.value.contains(event.target as Node)) {
            isOpen.value = false
        }
    }

    onMounted(() => {
        document.addEventListener('click', handleClickOutside)
    })

    onBeforeUnmount(() => {
        document.removeEventListener('click', handleClickOutside)
    })
</script>

<template>
  <div class="select" ref="select">
    <button 
      class="select-trigger"
      @click="toggleDropdown"
      :aria-expanded="isOpen"
    >
      <span class="select__item-current">{{ currentItem.title }}</span>
    </button>

    <Transition name="slide-down">
      <div 
        v-show="isOpen"
        class="select-dropdown"
        @click.stop
      >
        <div 
          v-for="option in props.options"
          :key="option.id"
          class="select-option"
          :class="{ 'selected': option.id === currentItem.id }"
          @click="() => {if (currentItem.id !== option.id) {
            selectItem(option)
            $emit('change', option)
          }}"
        >
          {{ option.title }}
        </div>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.select {
  position: relative;
  display: inline-block;
}

.select-trigger {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 40px;
  width: 40px;
  padding: 10px 10px;
  background: #f9f9f9;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  color: #0f0f0f;
  transition: all 0.3s ease;
}

.select-trigger:hover {
  background: #d0d0d0;
}

.select__item-current {
  font-size: 15px;
  font-weight: 700;
}

.select-dropdown {
  position: absolute;
  top: calc(100% + 8px);
  left: 0;
  width: 100%;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transform-origin: top center;
  z-index: 100;
}

.select-option {
  padding: 10px 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  font-size: 15px;
  font-weight: 700;
}

.select-option:hover {
  background: #d0d0d0;
}

.select-option.selected {
  background-color: rgba(40, 40, 40, 0.4);
  color: #f2f2f2;
  font-size: 15px;
  cursor: default;
}

.slide-down-enter-active,
.slide-down-leave-active {
  transition: 
    transform 0.3s cubic-bezier(0.4, 0, 0.2, 1),
    opacity 0.2s ease;
}

.slide-down-enter-from,
.slide-down-leave-to {
  opacity: 0;
  transform: scaleY(0.8) translateY(-10px);
}
</style>