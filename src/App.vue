<script setup lang="ts">
import HeaderApp from './components/HeaderApp.vue'
import { onMounted, onUnmounted, ref } from 'vue'

enum Languages {
  RU = 'RU',
  EN = 'EN',
  FR = 'FR',
  DE = 'DE',
}

const cartPrice = ref<number>(0);
const showCart = ref<boolean>(false);
const isScrolled = ref(false);
const currentLanguage = ref(Languages.RU)

const languages = [
  { id: 0, title: 'RU', value: Languages.RU },
  { id: 1, title: 'EN', value: Languages.EN },
  { id: 2, title: 'FR', value: Languages.FR },
  { id: 3, title: 'DE', value: Languages.DE }
]

const handleScroll = () => {
  isScrolled.value = window.scrollY > 10
}

onMounted(() => {
  setTimeout(() => {
    cartPrice.value = 14500;
  }, 300)
  window.addEventListener('scroll', handleScroll);
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
})

const toggleCart = (isOpen: boolean) => {
  showCart.value = !isOpen;
}
</script>

<template>
  <HeaderApp 
    :cartPrice="cartPrice"
    :language-list="languages"
    @toggle-cart="toggleCart"
    @select-lang="(lang) => currentLanguage = lang?.value"
    :show-cart="showCart"
    class="header"
    :class="{ 'scrolled': isScrolled, 'cart--opened': showCart }"
  />
  <main :class="{ 'main--cart-opened': showCart }">
    <div class="menu" :class="{'menu--cart-close': !showCart, 'menu--cart-open': showCart}">
      <h1>Суши и роллы</h1>
      <div class="menu__content"></div>
    </div>
    <div class="cart" :class="{'cart--open': showCart}">
      <h1 class="cart__title">Мой заказ</h1>
      <div class="cart__content"></div>
    </div>
  </main>
</template>

<style scoped>
.header {
  position: sticky;
  top: 0;
  z-index: 11;
}

main {
  margin: 0 auto;
  padding: 0 50px;
  max-width: 1300px;
  min-width: 1140px;
  display: flex;
  position: relative;
  overflow-x: clip;
  transition: all 0.8s ease;
}

@media (min-width: 1691px) {
  main.main--cart-opened {
    max-width: 1691px;
  }
}

@media (min-width: 1400px) and (max-width: 1691px) {
  main.main--cart-opened {
    max-width: 100%;
  }
}

.menu {
  flex: 1;
  height: 2500px;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.menu__content {
  background: #f5f5f5;
  width: 100%;
  height: 100%;
}

@keyframes resizeMenu {
  0% {
    margin-right: -391px;
  }
  8% {
    margin-right: -391px;
  }
  100% {
    margin-right: 0px;
  }
}

@keyframes resizeMenuReverse {
  0% {
    margin-right: 0px;
  }
  82% {
    margin-right: -391px;
  }
  100% {
    margin-right: -391px;
  }
}

.menu--cart-open {
  animation: resizeMenu 0.8s ease normal;
}

.menu--cart-close {
  margin-right: -391px;
  animation: resizeMenuReverse 0.8s ease normal;
}

.cart {
  width: 375px;
  height: calc(100vh - 145px);
  margin-left: 15px;
  background: #fff;
  border-left: 1px solid rgba(0, 0, 0, 0.3);
  transform: translateX(125%);
  transition: transform 0.8s ease;
  display: flex;
  flex-direction: column;
  gap: 15px;
  position: sticky;
  top: 145px;
}

.cart--open {
  transform: translateX(0);
}

.cart__title {
  margin-left: 15px;
}

.cart__content {
  margin-left: 15px;
  background: #f5f5f5;
  height: 700px;
}
</style>
