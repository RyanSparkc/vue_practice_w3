<template>
  <div v-if="!order.id" class="alert alert-secondary text-center" role="alert">
    尚未建立訂單
  </div>
  <div v-else class="card">
    <div class="card-body">
      <div class="card-title">
        <h5>訂單</h5>
        <table class="table">
          <thead>
            <tr>
              <th scope="col">品項</th>
              <th scope="col">數量</th>
              <th scope="col">小計</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="orderItem in props.order.cart" :key="orderItem.id">
              <td>{{ orderItem.name }}</td>
              <td>{{ orderItem.quantity }}</td>
              <td>{{ orderItem.price * orderItem.quantity }}</td>
            </tr>
          </tbody>
        </table>
        <div class="text-end">備註: <span>{{ order.description }}</span></div>
        <div class="text-end">
          <h5>總計: <span>{{ order.total }}</span></h5>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>

const props = defineProps({
  order: {
    type: Object,
    required: true,
  },
});
</script>

<style lang="scss" scoped></style>