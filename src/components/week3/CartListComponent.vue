<template>
  <table class="table">
    <thead>
      <tr>
        <th scope="col" width="50">操作</th>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col" width="140">數量</th>
        <th scope="col">單價</th>
        <th scope="col">小計</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cartItem in props.cart" :key="cartItem.id" class="align-middle">
        <td><button type="button" class="btn btn-sm" @click="deleteCart(cartItem)">x</button></td>
        <td>{{ cartItem.name }}</td>
        <td><small>{{ cartItem.description }}</small></td>
        <td>
          <select class="form-select" v-model="cartItem.quantity" @change="updateCart(cartItem)">
            <option v-for="i in 10" :key="i" :value="i">{{ i }}</option>
          </select>
        </td>
        <td>{{ cartItem.price }}</td>
        <td>{{ cartItem.price * cartItem.quantity }}</td>
      </tr>

    </tbody>
  </table>
  <div v-if="cart.length === 0" class="alert alert-primary text-center" role="alert">
    請選擇商品
  </div>
  <div v-else>
    <div class="text-end mb-3">
      <h5>總計: <span>{{ totalSum }}</span></h5>
    </div>
    <textarea class="form-control mb-3" rows="3" placeholder="備註" v-model="cart_note"></textarea>
    <div class="text-end">
      <button class="btn btn-primary" @click="submitOrder">送出</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  cart: {
    type: Array,
    required: true,
  },
  totalSum: {
    type: Number,
    required: true,
  },
  description: {
    type: String,
  },
});

const emit = defineEmits(['updateCart', 'deleteCart', 'submitOrder']);

const updateCart = (cartItem) => {
  emit('updateCart', cartItem);
};

const deleteCart = (cartItem) => {
  emit('deleteCart', cartItem);
};


const cart_note = ref('');
const submitOrder = () => {
  emit('submitOrder', cart_note.value);
  cart_note.value = '';
};





</script>

<style lang="scss" scoped></style>