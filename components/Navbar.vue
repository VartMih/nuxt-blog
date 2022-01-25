<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">
    <nuxt-link class="navbar-brand" to="/">
      <img src="https://upload.wikimedia.org/wikipedia/commons/c/c3/Python-logo-notext.svg" width="30" height="30" alt="logo">
    </nuxt-link>
    <nuxt-link class="navbar-brand" to="/">
      <img src="https://nuxtjs.org/design-kit/colored-logo.svg" width="30" height="30" alt="logo">
    </nuxt-link>
    <button class="navbar-toggler" type="button" data-toggle="collapse"
            data-target="#navbarSupportedContent"
            aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active">
          <nuxt-link class="nav-link" to="/">Главная</nuxt-link>
        </li>
        <li class="nav-item ">
          <nuxt-link class="nav-link" to="/contact">Контакты</nuxt-link>
        </li>
      </ul>
      <form class="form-inline my-2 my-lg-0">
        <input v-if="variable != 'search'" name="q" v-model="q" type="text" class="form-control mr-sm-2" placeholder="Поиск" aria-label="Поиск">
        <button v-if="variable != 'search'" class="btn btn-outline-success my-2 my-sm-0 mr-5" type="submit" @click.stop.prevent="submit()">Поиск</button>
      </form>
      <nuxt-link title="Профиль пользователя" :to="`/account`"><img v-if="user" src="/img/avatar.png" width="40" height="40" alt="avatar"></nuxt-link>
        <nuxt-link class="username" title="Профиль пользователя" :to="`/account`" v-if="user">{{user.username}}</nuxt-link>
      <span v-if="loggedIn"><nuxt-link class="btn btn-outline-light mr-2" to="/signout">Выход</nuxt-link></span>
      <span v-else>
        <nuxt-link class="btn btn-outline-light mr-2" to="/signin">Вход</nuxt-link>
        <nuxt-link class="btn btn-outline-light mr-2" to="/signup">Регистрация</nuxt-link>
      </span>
    </div>
  </nav>
</template>

<script>
export default {
  props: ["variable"],
  data(){
    return {
      q : null,
    }
  },
  methods: {
    submit(){
      this.$router.push("/search?q="+this.q);
    }
  },
  computed: {
    loggedIn() {
      return this.$auth.loggedIn
    },
    user() {
      return this.$auth.user
    }
  }
}
</script>

<style scoped>

</style>