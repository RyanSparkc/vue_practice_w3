<template>
  <div class="d-flex justify-content-center align-items-center card-height-center">
    <div class="card" style="width: 350px">
      <div class="card-body">
        <form>
          <div class="mb-3">
            <label for="exampleInputEmail1" class="form-label">Email address</label>
            <input type="email" class="form-control" id="exampleInputEmail1"
              aria-describedby="emailHelp">
          </div>
          <div class="mb-3">
            <label for="exampleInputPassword1" class="form-label">Password</label>
            <input type="password" class="form-control" id="exampleInputPassword1">
          </div>
          <div class="mb-3">
            <label for="exampleInputNickname" class="form-label">Nickname</label>
            <input type="text" class="form-control" id="exampleInputNickname">
          </div>
          <button type="submit" class="btn btn-primary d-block w-100">註冊</button>
          <router-link to="/login" class="d-block w-100 text-center mt-2">已經有帳號嗎?</router-link>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>

</script>

<style lang="scss" scoped></style>