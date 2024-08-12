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
        <div id="LoginReturnMessage" style="color:red; word-break: break-all; max-width: auto;">{{ token }}</div>
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
  <div class="container" v-if="showtodo">
    <div class="row">
      <div class="col-md-6">
        <h1>TODOLIST</h1>

        <form @submit.prevent="addtodo">
          <div class="mb-3">
            <label for="addtodo" class="form-label">add todo</label>
            <input type="text" id="addtodo" class="form-control" v-model="addTodo">
            <button type="submit" class="btn btn-primary">add</button>
          </div>
        </form>
        <div class="mb-3" v-for="item in todo" :key="item.id">
          <p v-if="!item.editing">
            {{ item.content }}
            <span class="ms-2 text-muted">{{ item.status ? '完成' : '未完成' }}</span>
            <button class="btn btn-sm btn-primary ms-3" @click="enableEditing(item)">编辑</button>
            <button class="btn btn-sm btn-danger ms-2" @click="deleteTodo(item)">删除</button>
          </p>
          <div v-else>
            <input type="text" v-model="item.updatedContent" class="form-control" />
            <select v-model="item.updatedStatus" class="form-select mt-2">
              <option :value="true">完成</option>
              <option :value="false">未完成</option>
            </select>
            <button class="btn btn-sm btn-success mt-2" @click="saveEdit(item)">保存</button>
            <button class="btn btn-sm btn-secondary mt-2 ms-2" @click="cancelEditing(item)">取消</button>
          </div>
        </div>
      </div>
    </div>
  </div>




</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
//signup
const email1 = ref('');
const Password1 = ref('');
const nickname = ref('');
//login
const email2 = ref('');
const Password2 = ref('');
const token = ref('');
const checktoken = ref('');
//todo
const showtodo = ref(false);
const todo = ref([]);
const addTodo = ref('');


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

    alert("註冊成功");
    email1.value = '';
    Password1.value = '';
    nickname.value = '';

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
    email2.value = '';
    Password2.value = '';
    showtodo.value = true;
    gettodo();
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
    const res = await axios.post(`${apiUrl}/users/sign_out`, {}, {
      headers: {
        Authorization: token.value
      }
    });
    alert("登出成功");
    token.value = '';
    showtodo.value = false;
  } catch (error) {
    console.log(error);
    alert("登出失敗: " + error);
  }

}

//todolist

const gettodo = async () => {
  try {
    const res = await axios.get(`${apiUrl}/todos/`, {
      headers: {
        Authorization: token.value
      }
    });
    todo.value = res.data.data;

  } catch (error) {
    console.log(error);
    alert("失敗: " + error);
  }
}

const addtodo = async () => {
  try {
    const res = await axios.post(`${apiUrl}/todos/`, {
      content: addTodo.value
    }, {
      headers: {
        Authorization: token.value
      }
    });
    if (res.data.status) {
      alert("success");
      gettodo();
      console.table(todo.value);
    } else {
      alert("失敗");
    }
    console.table(todo.value);
  } catch (error) {
    console.log(error);
    alert("失敗: " + error);
  }
};

const enableEditing = (item) => {
  item.editing = true;
  item.updatedContent = item.content;
  item.updatedStatus = item.status;
};
const cancelEditing = (item) => {
  item.editing = false;
};
const saveEdit = async (item) => {
  try {
    const res = await axios.put(`${apiUrl}/todos/${item.id}`, {
      content: item.updatedContent,
      status: item.updatedStatus
    }, {
      headers: {
        Authorization: token.value
      }
    });
    if (res.data.status) {
      item.content = item.updatedContent;
      item.status = item.updatedStatus;
      item.editing = false;
      console.log(res.data.message);
    } else {
      console.error('更新失敗:', res.data.message);
    }
  } catch (error) {
    console.error('编辑失败:', error);
  }
};
const deleteTodo = async (item) => {
 
  try {
    const res = await axios.delete(`${apiUrl}/todos/${item.id}`, {
      headers: {
        "Authorization": token.value,
        "Content-type": 'application/json'
      }
    })
    if (res.data.status) {
      console.log(res.data.message); 
      alert(res.data.message);
      todo.value = todo.value.filter(t => t.id !== item.id);
    } else {
      console.error('刪除失敗:', res.data.message);
      alert('刪除失敗: ' + res.data.message);
    }
  } catch (error) {
    console.log(error);
  }
}


</script>

<style>
body {
  background-color: #ffffff;
}
</style>