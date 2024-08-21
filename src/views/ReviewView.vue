<template>
  <h1>Review</h1>
  <div>
    <input type="text" v-model="newName"> {{ newName }}
    <input type="number" v-model="newNumber"> {{ newNumber }}
    <button type="button" @click="addProduct">新增到資料集裡面</button>
  </div>
  <div>
    {{ data }}
    <table>
      <thead>
        <tr>
          <th>標題</th>
          <th>價格</th>
          <th>調整價格</th>
          <th>刪除</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in data" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.price }}</td>
          <td>
            <input type="number" v-model="item.price">
          </td>
          <td>
            <button type="button" @click="deleteItem(item)">刪除品項</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <div>
    <h3>總額: {{ total }}</h3>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const newName = ref('');
const newNumber = ref(0);
const data = ref([]);

const addProduct = () => {
  data.value.push({
    id: new Date().getTime(),
    name: newName.value,
    price: newNumber.value
  });
  newName.value = '';
  newNumber.value = 0;
}

const deleteItem = (item) => {
  data.value = data.value.filter((i) => i.id !== item.id);
}

const total = computed(() => {
  return data.value.reduce((acc, item) => acc + item.price, 0);
});

onMounted(() => {
  data.value = [
    { id: 1, name: 'Apple', price: 10 },
    { id: 2, name: 'Banana', price: 20 },
    { id: 3, name: 'Cherry', price: 30 },
    { id: 4, name: 'Durian', price: 40 },
    { id: 5, name: 'Elderberry', price: 50 }
  ];
});












</script>

<style lang="scss" scoped></style>