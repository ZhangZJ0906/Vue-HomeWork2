<template>


  <div class="container">
    <div class="row">
      <div class="col-md-6">
        <h1>註冊</h1>
        <form>
          <div class="mb-3 my-4">
            <label for="email1" class="form-label">email</label>
            <input type="text" id="email1" placeholder="email" class="form-control">
          </div>
          <div class="mb-3">
            <label for="Password1" class="form-label">Password</label>
            <input type="password" id="Password1" class="form-control" placeholder="password">
          </div>
          <div class="mb-3">
            <label for="nickname" class="form-label">nickname</label>
            <input type="password" id="nickname" class="form-control" placeholder="nickname">
          </div>
        </form>
        <div id="SignupReturnMessage"></div>
      </div>

      <div class="col-md-6">
        <h1>登入</h1>
        <form>
          <div class="mb-3">
            <label for="email2" class="form-label">email</label>
            <input type="text" id="email2" placeholder="email" class="form-control">
          </div>
          <div class="mb-3">
            <label for="Password2" class="form-label">Password</label>
            <input type="password" id="Password2" class="form-control" placeholder="password">
          </div>
        </form>
        <div id="LoginReturnMessage"></div>
      </div>
    </div>
  </div>


</template>

<script>
export default {
  name: 'App'

}
</script>

<style>
body {
  background-color: #ffffff;
}
</style>