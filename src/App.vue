<template>
  <div class="container">
    <div class="row">
      <div class="col-md-3">
        <!-- signup -->
        <h1>註冊</h1>
        <form @submit.prevent="signup">
          <div class="mb-3 my-4">
            <label for="email1" class="form-label">email</label>
            <input v-model="email1" type="text" id="email1" placeholder="email" class="form-control">
          </div>
          <div class="mb-3">
            <label for="Password1" class="form-label">Password</label>
            <input v-model="Password1" type="password" id="Password1" class="form-control" placeholder="password">
          </div>
          <div class="mb-3">
            <label for="nickname" class="form-label">nickname</label>
            <input v-model="nickname" type="text" id="nickname" class="form-control" placeholder="nickname">
          </div>
          <button type="submit" class="btn btn-primary">註冊</button>
        </form>
        <div id="SignupReturnMessage"></div>
      </div>
      <!-- login -->
      <div class="col-md-3">
        <h1>登入</h1>
        <form @submit.prevent="signin">
          <div class="mb-3">
            <label for="email2" class="form-label">email</label>
            <input v-model="email2" type="text" id="email2" placeholder="email" class="form-control">
          </div>
          <div class="mb-3">
            <label for="Password2" class="form-label">Password</label>
            <input v-model="Password2" type="password" id="Password2" class="form-control" placeholder="password">
          </div>
          <button type="submit" class="btn btn-primary">登入</button>
        </form>
        <div id="LoginReturnMessage">{{ token }}</div>
      </div>
      <!-- check -->
      <div class="col-md-3">
        <h1>check</h1>
        <form @submit.prevent="check">
          <div class="mb-3">
            <label for="check">check token</label>
            <input type="text" id="check" v-model="checktoken" class="form-control">
          </div>
          <button type="submit" class="btn btn-primary">check</button>
        </form>
      </div>

      <!-- signout -->
      <div class="col-md-3">
        <h1>登出</h1>
        <form @submit.prevent="signout">
          <button type="submit" class="btn btn-primary">登出啦</button>
        </form>
      </div>
    </div>
  </div>

  <!-- todolist -->
  <!--  
  <div class="container" v-if="aa">
    <div class="row">
      <div class="col-md-12">
        <h1>TODOLIST</h1>
      </div>
    </div>
  </div> -->
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
const email1 = ref('');
const Password1 = ref('');
const nickname = ref('');
const email2 = ref('');
const Password2 = ref('');
const token = ref('');
const checktoken = ref('');
const apiUrl = 'https://todolist-api.hexschool.io';
console.table({
  account: "aaa1@gamil.com",
  password: "test123"
})

const signup = async () => {
  try {
    const res = await axios.post(`${apiUrl}/users/sign_up`, {
      email: email1.value,
      password: Password1.value,
      nickname: nickname.value
    });
    console.table(res);
    alert("註冊成功");
  } catch (error) {
    console.log(error);
    alert("註冊失敗");
  }
}

const signin = async () => {
  try {
    const res = await axios.post(`${apiUrl}/users/sign_in`, {
      email: email2.value,
      password: Password2.value,
    });
    alert("登入成功");
    token.value = res.data.token;
    // console.table(res.data.token);
  } catch (error) {
    console.log(error);
    alert("登入失敗: " + error);
  }
}

const check = async () => {
  try {
    // console.table(checktoken.value);
    const res = await axios.get(`${apiUrl}/users/checkout`, {
      headers: {
        Authorization: checktoken.value
      }
    });
    alert("success");
  } catch (error) {
    console.log(error);
    alert("失敗: " + error);
  }
}

const signout = async () => {
  try {
    console.log(token.value,apiUrl);
    const res = await axios.post(`${apiUrl}/users/sign_out`, {
      headers: {
        Authorization : token.value
      }
    });
    
    
    alert("登出成功");
  } catch (error) {
    console.log(error);
    alert("登出失敗: " + error);
  }

}

// const showTodoList = (aaa) => {
//   return aaa;
// }
// const aa = showTodoList(false);



</script>

<style>
body {
  background-color: #ffffff;
}
</style>