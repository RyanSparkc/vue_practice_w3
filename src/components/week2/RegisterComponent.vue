<template>
  <div class="d-flex justify-content-center align-items-center card-height-center">
    <div class="card" style="width: 350px">
      <pre>{{ userInfo }}</pre>
      <div class="card-body">
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
          <div class="mb-3">
            <label for="exampleInputNickname" class="form-label">Nickname</label>
            <input type="text" class="form-control" id="exampleInputNickname"
              v-model="userInfo.nickname">
          </div>
          <button type="button" class="btn btn-primary d-block w-100" @click="register">註冊</button>
          <router-link to="/week2/login"
            class="d-block w-100 text-center mt-2">已經有帳號嗎?</router-link>
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

const userInfo = ref({
  email: '',
  password: '',
  nickname: '',
});

const register = async () => {
  try {
    const res = await axios.post('https://todolist-api.hexschool.io/users/sign_up', userInfo.value);
    console.log(res);
    router.push('/week2/login');
  } catch (error) {
    console.log(error);
  }
};
</script>

<style lang="scss" scoped></style>