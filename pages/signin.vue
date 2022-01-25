<template>
  <div class="signin-top">
    <div class="card text-white bg-dark mb-3 signin" style="max-width: 28rem;">
      <form class="form-signin" @submit.prevent="userLogin">
        <div class="card-header card-signin">Авторизация</div>
        <div class="card-body">
          <label for="inputUsername" class="sr-only">Имя пользователя</label>
          <input id="inputUsername" class="form-control signin-form" placeholder="Имя пользователя" required="" v-model="login.username">
          <label for="inputPassword" class="sr-only">Пароль</label>
          <input type="password" id="inputPassword" class="form-control mt-3 signin-form" placeholder="Пароль" required="" v-model="login.password">
          <button class="btn mt-4 btn-outline-success btn-block form-btn" type="submit">Войти</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  layout: "post_detail",
  data() {
    return {
      login: {
        username: '',
        password: ''
      },
    }
  },
  methods: {
    async userLogin() {
      try {
        let response = await this.$auth.loginWith('local', { data: this.login })
        this.$router.back()
      } catch (err) {
        alert("Неверная пара логин/пароль")
      }
    }
  }
}
</script>

<style scoped>
</style>