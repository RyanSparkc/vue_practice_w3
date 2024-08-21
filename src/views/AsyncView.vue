<template>
  <h1>Async Function</h1>
</template>

<script setup>

import axios from 'axios'

// console.log(axios);

// axios.get('https://randomuser.me/api/')
//   .then(response => {
//     console.log(response);
//     return axios.get('https://randomuser.me/api/')
//   })
//   .then(response => {
//     console.log(response);
//   })
//   .catch(error => {
//     console.error(error.message);
//   });


async function getUser() {
  console.log('getUser');
  // 定義要取得什麼資料、執行什麼方法
  try {
    const response = await axios.get('https://randomuser.me/api/')
    console.log(response);
  } catch (error) {
    console.error(error.message);
  }
}

getUser()




</script>