<template>
  <div>
    <loading :active.sync="isLoading"></loading>
    <div class="detail-bg py-5">
      <form class="form-signin py-5" @submit.prevent.enter="signin">
        <h1 class="h3 mb-3 font-weight-normal text-center">請登入</h1>
        <label for="inputEmail" class="h4">帳號</label>
        <input type="email" id="inputEmail" v-model="user.username" class="form-control" placeholder="Email address" required autofocus>
        <label for="inputPassword" class="mt-3 h4">密碼</label>
        <input type="password" id="inputPassword" v-model="user.password" class="form-control" placeholder="Password" required>
        <!-- <div class="checkbox mb-3">
          <label>
            <input type="checkbox" value="remember-me"> 記住我
          </label>
        </div> -->
        <button class="btn btn-lg btn-light btn-block" type="submit">Sign in</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      user: {
        username: '',
        password: ''
      },
      isLoading: false
    }
  },
  methods: {
    signin () {
      const api = `${process.env.VUE_APP_APIPATH}/admin/signin`
      const vm = this
      vm.isLoading = true
      vm.$http.post(api, vm.user).then((response) => {
        if (response.data.success) {
          vm.$router.push('/admin/products')
          vm.isLoading = false
        }
      })
    }
  }
}
</script>

<style scoped>
html,
body {
  height: 100%;
}

body {
  display: -ms-flexbox;
  display: flex;
  -ms-flex-align: center;
  align-items: center;
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #f5f5f5;
}

.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}
.form-signin .checkbox {
  font-weight: 400;
}
.form-signin .form-control {
  position: relative;
  box-sizing: border-box;
  height: auto;
  padding: 10px;
  font-size: 16px;
}
.form-signin .form-control:focus {
  z-index: 2;
}
.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>
