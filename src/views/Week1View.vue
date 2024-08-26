<template>
  <div>
    <h1>商品庫存</h1>
    <button class="btn btn-primary mb-3 float-end mb-4" @click="addItem">新增商品</button>
    <table class="table table-hover mb-5">
      <thead>
        <tr>
          <th scope="col">品項</th>
          <th scope="col">描述</th>
          <th scope="col">價格</th>
          <th scope="col">庫存</th>
          <th scope="col">編輯</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in data" :key="item.id">
          <td scope="row">
            <span v-if="!item.isEdit">{{ item.title }}</span>
            <input v-else class="form-control" type="text" v-model="tempData.title">
          </td>
          <td>
            <span v-if="!item.isEdit">{{ item.desc }}</span>
            <input v-else class="form-control" type="text" v-model="tempData.desc">
          </td>
          <td>
            <span v-if="!item.isEdit">{{ item.price }}</span>
            <input v-else class="form-control" type="number" v-model.number="tempData.price">
          </td>
          <td>
            <div v-if="!item.isEdit">
              <button class="btn btn-sm btn-light" @click="addCount(item)">+</button>
              <span class="mx-2">{{ item.count }}</span>
              <button class="btn btn-sm btn-light" @click="subCount(item)">-</button>
            </div>
            <input v-else class="form-control" type="number" v-model.number="tempData.count">
          </td>
          <td class="d-flex gap-2">
            <button v-if="!item.isEdit" class="btn btn-sm btn-primary"
              @click="editItem(item)">編輯</button>
            <template v-else>
              <button class="btn btn-sm btn-success" @click="confirmEdit">確認</button>
              <button class="btn btn-sm btn-danger" @click="cancelEdit(item)">取消</button>
            </template>

          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="isAdd">
      <h2>新增商品</h2>
      <div class="card">
        <div class="card-body">
          <div class="row">
            <div class="col">
              <label class="form-label" for="new-title">品項</label>
              <input class="form-control" id="new-title" type="text" v-model="tempData.title">
            </div>
            <div class="col">
              <label class="form-label" for="new-desc">描述</label>
              <input class="form-control" id="new-desc" type="text" v-model="tempData.desc">
            </div>
            <div class="col">
              <label class="form-label" for="new-price">價格</label>
              <input class="form-control" id="new-price" type="number" v-model="tempData.price">
            </div>
            <div class="col">
              <label class="form-label" for="new-count">庫存</label>
              <input class="form-control" id="new-count" type="number" v-model="tempData.count">
            </div>
            <div class="col align-self-end d-flex gap-2">
              <button class="btn btn-primary" @click="confirmAdd">確認新增</button>
              <button class="btn btn-danger" @click="isAdd = false">取消</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const data = ref([
  {
    id: 1,
    title: '珍珠奶茶',
    desc: '香濃奶茶搭配QQ珍珠',
    price: 50,
    count: 20,
    isEdit: false
  },
  {
    id: 2,
    title: '冬瓜檸檬',
    desc: '清新冬瓜配上新鮮檸檬',
    price: 45,
    count: 18,
    isEdit: false
  },
  {
    id: 3,
    title: '翡翠檸檬',
    desc: '綠茶與檸檬的完美結合',
    price: 55,
    count: 34,
    isEdit: false
  },
  {
    id: 4,
    title: '四季春茶',
    desc: '香醇四季春茶，回甘無比',
    price: 45,
    count: 10,
    isEdit: false
  },
  {
    id: 5,
    title: '阿薩姆奶茶',
    desc: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    count: 25,
    isEdit: false
  },
  {
    id: 6,
    title: '檸檬冰茶',
    desc: '檸檬與冰茶的清新組合',
    price: 45,
    count: 20,
    isEdit: false
  },
  {
    id: 7,
    title: '芒果綠茶',
    desc: '芒果與綠茶的獨特風味',
    price: 55,
    count: 18,
    isEdit: false
  },
  {
    id: 8,
    title: '抹茶拿鐵',
    desc: '抹茶與鮮奶的絕配',
    price: 60,
    count: 20,
    isEdit: false
  }
])

const isAdd = ref(false)

const tempData = ref({
  id: '',
  title: '',
  desc: '',
  price: '',
  count: ''
})

// 編輯按鈕，點選後修改該項狀態並且可以編輯資料
const editItem = (item) => {
  tempData.value = item;
  tempData.value.isEdit = true;
  // console.log(tempData.value)
}

// 確認編輯
const confirmEdit = () => {
  console.log('confirm-tempData', tempData.value)
  tempData.value.isEdit = false;

  const itemToUpdate = data.value.find(item => item.id === tempData.value.id);
  if (itemToUpdate) {
    Object.assign(itemToUpdate, tempData.value);
  }

  console.log('confirm-data', data.value)

}

// 增加商品數量
const addCount = (item) => {
  const itemToUpdate = data.value.find(dataItem => dataItem.id === item.id);
  if (itemToUpdate) {
    itemToUpdate.count++;
  }
  console.log('addCount-data', data.value)
}

// 減少商品數量
const subCount = (item) => {
  const itemToUpdate = data.value.find(dataItem => dataItem.id === item.id);
  if (itemToUpdate && itemToUpdate.count > 0) {
    itemToUpdate.count--;
  }
  console.log('subCount-data', data.value)
}

// 取消編輯
const cancelEdit = (item) => {
  tempData.value = item;
  tempData.value.isEdit = false;
  console.log('cancel-tempData', tempData.value)
}

// 新增商品
const addItem = () => {
  isAdd.value = true;
  tempData.value = {
    id: '',
    title: '',
    desc: '',
    price: '',
    count: ''
  }
}

// 確認新增
const confirmAdd = () => {
  // 使用 Date.now() 生成唯一 ID
  const newId = Date.now();

  // 將新 ID 添加到 tempData 中
  const newItem = { ...tempData.value, id: newId, isEdit: false };

  console.log('confirm-tempData', newItem);
  isAdd.value = false;
  data.value.push(newItem);
  console.log('confirm-data', data.value);


  // 重置 tempData
  tempData.value = {
    id: '',
    title: '',
    desc: '',
    price: '',
    count: ''
  };
}


</script>

<style lang="scss" scoped></style>