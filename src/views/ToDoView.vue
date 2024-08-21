<template>
  <h1>
    ToDo API
  </h1>
  <div>
    <h2>註冊</h2>
    <input type="email" placeholder="email" v-model="signupFields.email">
    <input type="password" placeholder="password" v-model="signupFields.password">
    <input type="text" placeholder="nickname" v-model="signupFields.nickname">
    <br>
    {{ signupFields }}
    <button type="button" @click="signup">註冊</button>
    {{ signupMessage }}
  </div>
  <div>
    <h2>登入</h2>
    <input type="email" placeholder="email" v-model="signInFields.email">
    <input type="password" placeholder="password" v-model="signInFields.password">
    <br>
    {{ signInFields }}
    <button type="button" @click="signIn">登入</button>
    <p>Token ： {{ signInToken }}</p>
  </div>

  <!-- 驗證 -->
  <div>
    <h2>驗證</h2>
    <div v-if="userInfo.uid">
      <p>UID: {{ userInfo.uid }}</p>
      <p>nickname: {{ userInfo.nickname }}</p>
    </div>
    <div v-else>
      <p>尚未登入</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
const apiUrl = 'https://todolist-api.hexschool.io';

const signupFields = ref({
  email: '',
  password: '',
  nickname: ''
})
const signupMessage = ref('')

const signup = async () => {
  console.log(`${apiUrl}/users/sign_up`)
  try {
    const res = await axios.post(`${apiUrl}/users/sign_up`, signupFields.value)
    console.log(res);
    signupMessage.value = res.data.uid
  } catch (error) {
    console.log(error.response.data.message);
  }
}

// 登入
const signInFields = ref({
  email: '',
  password: ''
})

const signInToken = ref('')


const signIn = async () => {
  try {
    const res = await axios.post(`${apiUrl}/users/sign_in`, signInFields.value)
    signInToken.value = res.data.token
    // 修正 cookie 的設置

    const expirationDate = new Date(res.data.exp * 1000).toUTCString();

    document.cookie = `Vue-Token=${res.data.token}; expires=${expirationDate}; path=/`;
    // console.log('exp', new Date(res.data.exp * 1000));
  } catch (error) {
    console.log(error.response.data.message);
  }
}


// 驗證
const userInfo = ref({
  nickname: '',
  uid: ''
})
onMounted(async () => {
  const myCookie = document.cookie.replace(
    /(?:(?:^|.*;\s*)Vue-Token\s*=\s*([^;]*).*$)|^.*$/,
    "$1",
  );
  console.log('cookie', myCookie);

  const res = await axios.get(`${apiUrl}/users/checkout`, {
    headers: {
      Authorization: myCookie
    }
  })
  console.log(res);
  userInfo.value = res.data
})
</script>

<style lang="scss" scoped></style>