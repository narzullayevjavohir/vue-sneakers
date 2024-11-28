<script setup>
defineProps({
  totalPrice: Number,
})

const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-300 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Snkeakers</h2>
          <p class="text-slate-400">Магазин лучших кроссовок</p>
        </div>
      </div>
    </router-link>
    <ul class="flex items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-3 text-slate-500 hover:text-black cursor-pointer"
      >
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} руб.</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center gap-3 text-slate-500 hover:text-black cursor-pointer">
          <img src="/heart.svg" alt="Heart" />
          <b>Закладки</b>
        </li>
      </router-link>
      <li class="flex items-center gap-3 text-slate-500 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="Profile" />
        <b>Профиль</b>
      </li>
    </ul>
  </header>
</template>
