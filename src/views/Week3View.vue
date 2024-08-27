<template>
  <div id="root">
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-4">
          <DrinksMenuComponent :drinkList="drinkList" @addCart="addCart" />
        </div>
        <div class="col-md-8">
          <CartListComponent :cart="cart" @updateCart="updateCart" @deleteCart="deleteCart"
            @submitOrder="submitOrder" :totalSum="totalSum" />
        </div>
      </div>
      <hr />
      <div class="row justify-content-center">
        <div class="col-8">
          <OrderListComponent :order="order" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import DrinksMenuComponent from '@/components/week3/DrinksMenuComponent.vue';
import CartListComponent from '@/components/week3/CartListComponent.vue';
import OrderListComponent from '@/components/week3/OrderListComponent.vue';

const data = [
  {
    "id": 1,
    "name": "珍珠奶茶",
    "description": "香濃奶茶搭配QQ珍珠",
    "price": 50
  },
  {
    "id": 2,
    "name": "冬瓜檸檬",
    "description": "清新冬瓜配上新鮮檸檬",
    "price": 45
  },
  {
    "id": 3,
    "name": "翡翠檸檬",
    "description": "綠茶與檸檬的完美結合",
    "price": 55
  },
  {
    "id": 4,
    "name": "四季春茶",
    "description": "香醇四季春茶，回甘無比",
    "price": 45
  },
  {
    "id": 5,
    "name": "阿薩姆奶茶",
    "description": "阿薩姆紅茶搭配香醇鮮奶",
    "price": 50
  },
  {
    "id": 6,
    "name": "檸檬冰茶",
    "description": "檸檬與冰茶的清新組合",
    "price": 45
  },
  {
    "id": 7,
    "name": "芒果綠茶",
    "description": "芒果與綠茶的獨特風味",
    "price": 55
  },
  {
    "id": 8,
    "name": "抹茶拿鐵",
    "description": "抹茶與鮮奶的絕配",
    "price": 60
  }
]
const drinkList = ref(data);
const cart = ref([]);



const addCart = (drink) => {
  const cartItem = cart.value.find((item) => item.name === drink.name);

  if (cartItem) {
    // 如果購物車中已有此飲料,檢查數量是否達到上限
    if (cartItem.quantity < 10) {
      cartItem.quantity++;
    } else {
      alert('此飲料已達購買上限10杯');
    }
  } else {
    // 如果購物車中沒有此飲料,新增到購物車
    cart.value.push({
      ...drink,
      quantity: 1,
      id: new Date().getTime()
    });
  }

  console.log('cart', cart.value);
}

const updateCart = (cartItem) => {
  cart.value = cart.value.map((item) => {
    if (item.id === cartItem.id) {
      return cartItem;
    }
    return item;
  });
}

// 計算總計
const totalSum = computed(() => {
  return cart.value.reduce((acc, curr) => acc + curr.price * curr.quantity, 0);
})



// 刪除購物車單一品項
const deleteCart = (cartItem) => {
  cart.value = cart.value.filter((item) => item.id !== cartItem.id);
}

// 送出訂單
const order = ref([]);
const submitOrder = (cart_note) => {
  order.value = {
    id: new Date().getTime(),
    description: cart_note,
    cart: cart.value,
    total: totalSum.value
  };
  cart.value = [];
  console.log('order', order.value);
}

</script>

<style lang="scss" scoped></style>