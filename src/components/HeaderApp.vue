<script setup lang="ts">
import CustomButton from './UI/CustomButton.vue';
import IconMenu from './icons/IconMenu.vue';
import IconCart from './icons/IconCart.vue';
import IconSearch from './icons/IconSearch.vue';
import CustomInput from './UI/CustomInput.vue';
import { onMounted, ref, watch } from 'vue';
const props = defineProps({
    cartPrice: {
        type: Number,
        required: false,
        default: 0
    },
    showCart: {
        type: Boolean,
        required: false,
        default: false
    }
});

const emit = defineEmits(['toggle-cart']);

const targetWidth = ref(0);

onMounted(() => {
  updateButtonWidth();
});

watch(
  () => props.cartPrice,
  () => {
    updateButtonWidth();
  }
);

function updateButtonWidth() {
  const tempSpan = document.createElement('span');
  tempSpan.style.visibility = 'hidden';
  tempSpan.style.position = 'absolute';
  tempSpan.style.whiteSpace = 'nowrap';
  tempSpan.innerText = props.cartPrice + ' ₸';
  document.body.appendChild(tempSpan);
  
  targetWidth.value = tempSpan.offsetWidth + 75;
  
  document.body.removeChild(tempSpan);
}
</script>

<template>
  <header>
    <div class="header__btns-group-left">
        <CustomButton>
            <IconMenu/>
        </CustomButton>
        <CustomButton>RU</CustomButton>
        <CustomInput></CustomInput>
    </div>
    <div class="header__cart-btn">
        <CustomButton
            :active="showCart"
            class="cart-btn"    
            :style="{ '--target-width': targetWidth + 'px' }"
            @click="$emit('toggle-cart', showCart)"
        >
            <IconCart class="cart-icon" />
            <div class="price-container">
                <Transition name="price-change">
                    <span class="price" :key="cartPrice">{{ cartPrice }}</span>
                </Transition>
            </div>
            <span class="currency">₸</span>
        </CustomButton>
    </div>
  </header>
</template>

<style scoped>
header {
    max-width: 1300px;
    min-width: 1140px;
    height: 85px;
    padding: 0 50px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.header__btns-group-left {
    display: flex;
    gap: 10px;
}

.header__btns-group-left button {
    height: 40px;
    width: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.header__cart-btn_content {
    display: flex;
    align-items: center;
    gap: 8px;
}

.header__cart-btn_content p {
    margin: 0;
}

.cart-btn {
  --initial-width: 120px;
  --target-width: var(--initial-width);
  
  display: inline-flex;
  gap: 8px;
  padding: 10px 15px;
  overflow: hidden;
  height: 40px;
  min-width: var(--initial-width);
  width: auto;
  transition: min-width 0.5s ease;
}

.cart-btn {
  min-width: var(--target-width);
}

.cart-icon {
  height: 20px;
}

.price-container {
  position: relative;
  display: flex;
  align-items: center;
  height: 20px;
}

.price {
  margin-right: 4px;
}

.currency {
  position: absolute;
  right: 15px;
  height: 20px;
}

.price-change-enter-active,
.price-change-leave-active {
  transition: all 0.5s ease;
  position: absolute;
}

.price-change-enter-from {
  transform: translateY(-100%);
  opacity: 0;
}

.price-change-enter-to {
  transform: translateY(0);
  opacity: 1;
}

.price-change-leave-from {
  transform: translateY(0);
  opacity: 1;
}

.price-change-leave-to {
  transform: translateY(100%);
  opacity: 0;
}

</style>
