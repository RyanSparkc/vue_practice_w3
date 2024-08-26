<template>
  <div>
    <h1 class="text-center mb-3">新增待辦事項</h1>
    <div class="d-flex justify-content-center">
      <div class="card w-50">
        <div class="card-header text-center">
          <h3 class="text-primary mb-0">week 2</h3>
        </div>
        <div class="card-body">
          <div>
            <div class="d-flex justify-content-between mb-3">
              <div class="w-100 d-flex gap-2">
                <input type="text" class="form-control" placeholder="Token" aria-label="Token"
                  aria-describedby="button-verify" v-model="myCookie">
                <button class="btn btn-outline-success btn-sm flex-shrink-0" type="button"
                  id="button-verify" @click="checkout">驗證</button>
                <button class="btn btn-outline-danger btn-sm flex-shrink-0" type="button"
                  id="button-logout" @click="signOut">登出</button>
              </div>
            </div>
            <hr>
            <div>
              <!-- 設定一個尚未驗證 -->
              <div v-if="loginStatus.uid === ''">
                <p>尚未驗證</p>
              </div>
              <div v-else-if="loginStatus.status">
                <p class="text-success">驗證成功</p>
                <p>UID: <strong class="text-success">{{ loginStatus.uid }}</strong></p>
                <p>Nickname: <strong class="text-success">{{ loginStatus.nickname }}</strong></p>
              </div>
              <div v-else-if="!loginStatus.status">
                <p class="text-danger">驗證失敗</p>
              </div>
              <!-- 設定一個尚未驗證 -->
            </div>
          </div>
          <hr>
          <div v-if="loginStatus.status">
            <form action="">
              <pre>{{ todo }}</pre>
              <div class="input-group mb-3">
                <input type="text" class="form-control" placeholder="輸入待辦事項" aria-label="輸入待辦事項"
                  aria-describedby="button-add" v-model="todo.content">
                <button class="btn btn-outline-primary" type="button" id="button-add"
                  @click="addTodo">新增</button>
              </div>
              <div class="card">
                <div class="card-body">
                  <table class="table">
                    <thead>
                      <tr>
                        <th scope="col" style="width: 20%;">待辦事項</th>
                        <th scope="col" style="width: 55%;">狀態</th>
                        <th scope="col" style="width: 25%;">操作</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr class="align-middle" v-for="item in inCompletedTodoList" :key="item.id">

                        <td>
                          <div class="form-check">
                            <input type="checkbox" class="form-check-input"
                              :id="`check-finish-${item.id}`" :checked="item.status"
                              @change="updateState(item.id)">
                            <label class="form-check-label" :for="`check-finish-${item.id}`">
                              {{ item.status ? '完成' : '未完成' }}
                            </label>
                          </div>
                        </td>
                        <td>
                          <input v-if="tempTodo.id === item.id && tempTodo.id !== ''" type="text"
                            class="form-control" placeholder="輸入待辦事項" v-model="tempTodo.content">
                          <p v-else class="mb-0">{{ item.content }}</p>
                        </td>
                        <td>
                          <div v-if="tempTodo.id !== item.id">
                            <button type="button" class="btn btn-sm btn-outline-primary me-2"
                              @click="editTodo(item.id)">編輯</button>
                            <button type="button" class="btn btn-sm btn-outline-danger"
                              @click="deleteTodo(item.id)">刪除</button>
                          </div>
                          <div v-else>
                            <button type="button" class="btn btn-sm btn-outline-success me-2"
                              @click="updateTodo(item.id)">更新</button>
                            <button type="button" class="btn btn-sm btn-outline-secondary"
                              @click="cancelEdit">取消</button>
                          </div>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </form>
          </div>
          <hr>
          <div v-if="loginStatus.status">
            <h3 class="text-success">已完成代辦事項</h3>
            <div class="card">
              <div class="card-body">
                <table class="table">
                  <tbody>
                    <tr class="align-middle text-decoration-line-through"
                      v-for="item in completedTodoList" :key="item.id">
                      <td>
                        <div class="form-check">
                          <input type="checkbox" class="form-check-input"
                            :id="`check-finish-${item.id}`" :checked="item.status"
                            @change="updateState(item.id)">
                          <label class="form-check-label" :for="`check-finish-${item.id}`">
                            {{ item.status ? '完成' : '未完成' }}
                          </label>
                        </div>
                      </td>
                      <td>
                        <input v-if="tempTodo.id === item.id && tempTodo.id !== ''" type="text"
                          class="form-control" placeholder="輸入待辦事項" v-model="tempTodo.content">
                        <p v-else class="mb-0">{{ item.content }}</p>
                      </td>
                      <td>
                        <!-- <div v-if="tempTodo.id !== item.id"> -->
                        <button type="button" class="btn btn-sm btn-outline-danger"
                          @click="deleteTodo(item.id)">刪除</button>
                        <!-- </div> -->
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, computed } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const router = useRouter();

const myCookie = ref('')

const apiUrl = 'https://todolist-api.hexschool.io';

const loginStatus = ref({
  nickname: '',
  uid: '',
  status: false
})


const checkout = async () => {
  try {
    const res = await axios.get(`${apiUrl}/users/checkout`, {
      headers: {
        Authorization: `${myCookie.value}`
      }
    })
    loginStatus.value.uid = res.data.uid;
    loginStatus.value.nickname = res.data.nickname;
    loginStatus.value.status = true;
    // console.log(loginStatus);
  } catch (error) {
    console.log(error.response.data);
    loginStatus.value.status = false;
    console.log(loginStatus.value);
  }
}

// 新增代辦事項
// Request failed with status code 400
const todo = ref({})
const addTodo = async () => {
  const res = await axios.post(`${apiUrl}/todos/`, {
    content: todo.value.content
  })
  todo.value = {}
  getTodo();
  console.log(res.data);
}

// 代辦事項列表
const todoList = ref([])

// 未完成代辦事項列表
const inCompletedTodoList = computed(() => {
  return todoList.value.data.filter(item => !item.status)
})

// 已完成代辦事項列表
const completedTodoList = computed(() => {
  return todoList.value.data.filter(item => item.status)
})

const getTodo = async () => {
  const res = await axios.get(`${apiUrl}/todos/`)
  todoList.value = res.data
  console.log('todo', todoList.value);
}


const tempTodo = ref({});
const originalTodo = ref({}); // 新增：用於存儲原始數據

// 編輯代辦事項
const editTodo = async (id) => {
  console.log('編輯', id);
  const todoToEdit = todoList.value.data.find(item => item.id === id);
  tempTodo.value = { ...todoToEdit }; // 創建一個副本
  originalTodo.value = { ...todoToEdit }; // 保存原始數據
  console.log('tempTodo', tempTodo.value);
}

// 取消編輯
const cancelEdit = () => {
  tempTodo.value = {}; // 恢復原始數據
  originalTodo.value = {}; // 清空原始數據

}

// 更新代辦事項
const updateTodo = async (id) => {
  console.log('更新', id);
  try {
    const res = await axios.put(`${apiUrl}/todos/${id}`, {
      content: tempTodo.value.content
    })
    // alert(res.data.message);
    getTodo();
    tempTodo.value = {}
    console.log(res.data);
  } catch (error) {
    console.log(error.response.data);
    alert(error.response.data.message);
  }
}

// 更新完成狀態
const updateState = async (id) => {
  console.log('更新', id);
  try {
    const res = await axios.patch(`${apiUrl}/todos/${id}/toggle`);
    console.log(res.data);
    await getTodo(); // 重新獲取所有待辦事項
    alert(res.data.message); // 顯示成功通知
  } catch (error) {
    console.error('更新失敗:', error);
    alert('更新失敗，請稍後再試。');
  }
}

// 刪除代辦事項
const deleteTodo = async (id) => {

  const res = await axios.delete(`${apiUrl}/todos/${id}`)
  console.log(res.data);
  alert(res.data.message);
  getTodo();
}

// 登出
const signOut = async () => {
  const res = await axios.post(`${apiUrl}/users/sign_out`)
  console.log(res.data);
  alert(res.data.message);
  loginStatus.value = {
    nickname: '',
    uid: '',
    status: false
  }
  myCookie.value = ''
  document.cookie = ''
  console.log('signOut', loginStatus.value);
  console.log('myCookie', myCookie.value);
  router.push('/login')
}




onMounted(() => {
  myCookie.value = document.cookie.replace(
    /(?:(?:^|.*;\s*)Vue-week2\s*=\s*([^;]*).*$)|^.*$/,
    "$1",
  );
  console.log('myCookie', myCookie.value);
  axios.defaults.headers.common['Authorization'] = `${myCookie.value}`;
  getTodo();
})

</script>

<style lang="scss" scoped></style>