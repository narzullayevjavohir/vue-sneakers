<script setup>
import InfoBlock from './InfoBlock.vue'
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import { ref, computed, inject } from 'vue'

const isCreating = ref(false)
const orderId = ref(null)

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
})

const { cart } = inject('cart')

const createOrder = async () => {
  try {
    isCreating.value = true

    const { data } = await axios.post(`https://55794da89abba706.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value,
    })

    cart.value = []

    orderId.value = data.id

    return data
  } catch (error) {
    console.log(error.message)
  } finally {
    isCreating.value = false
  }
}

const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
const cartIsEmpty = computed(() => cart.value.length === 0)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />
    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя вы один пару кроссовок, чтобы сделать заказ"
        image-url="/cart-empty.png"
      />
      <InfoBlock
        title="Заказ оформлен !"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
        v-if="orderId"
      />
    </div>

    <div v-else>
      <CartItemList v-if="totalPrice" />
      <div v-if="totalPrice" class="flex flex-col gap-4 my-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>

        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ +vatPrice }} ₽</b>
        </div>
      </div>
    </div>
    <button
      :disabled="buttonDisabled"
      @click="createOrder"
      class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-400 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
    >
      Оформить заказ
    </button>
  </div>
</template>
