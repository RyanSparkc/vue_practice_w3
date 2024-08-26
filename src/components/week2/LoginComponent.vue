<template>
  <div class="d-flex justify-content-center align-items-center card-height-center">
    <div class="card" style="width: 350px">
      <div class="card-body">
        {{ userInfo }}
        <form>
          <div class="mb-3">
            <label for="exampleInputEmail1" class="form-label">Email address</label>
            <input type="email" class="form-control" id="exampleInputEmail1"
              aria-describedby="emailHelp" v-model="userInfo.email">
          </div>
          <div class="mb-3">
            <label for="exampleInputPassword1" class="form-label">Password</label>
            <input type="password" class="form-control" id="exampleInputPassword1"
              v-model="userInfo.password">
          </div>
          <button type="button" class="btn btn-primary d-block w-100" @click="login">登入</button>
          <router-link to="/register" class="d-block w-100 text-center mt-2">沒有帳號嗎?</router-link>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const router = useRouter();

const sign_in = 'https://todolist-api.hexschool.io/users/sign_in';


const userInfo = ref({
  email: '',
  password: '',
});


const login = async () => {
  try {
    const res = await axios.post(sign_in, userInfo.value);
    console.log(res);
    const expirationDate = new Date(res.data.exp * 1000).toUTCString();
    document.cookie = `Vue-week2=${res.data.token}; expires=${expirationDate}; path=/`;
    router.push('/todo');
  } catch (err) {
    console.log(err);
    alert(err.response.data.message);
  }
};
</script>

<style lang="scss">
.card-height-center {
  min-height: calc(100vh - 180px);
}
</style>