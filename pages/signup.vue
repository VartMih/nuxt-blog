<template>
  <div class="register-top">
    <div class="card text-white bg-dark mb-3 register" style="max-width: 28rem;">
        <form class="form-register" @submit.prevent="userRegister">
          <div class="card-header card-register">Регистрация нового пользователя</div>
          <div class="card-body card-body-register">
            <div class="col-md-8" v-bind:class="{ 'fld-error': $v.register.username.$error }">
              <div class="md-form mb-0">
                <label for="inputUsername" class="sr-only">Имя пользователя</label>
                <input type="text" id="inputUsername" class="form-control" placeholder="Имя пользователя" v-model="register.username" @input="$v.register.username.$touch()">
                <span class="msg-error" v-if="!$v.register.username.required">
                  <small>Поле обязательно для заполнения</small>
                </span>
                <span class="msg-error" v-if="!$v.register.username.minLength">
                  <small>Должно быть не меньше {{ $v.register.username.$params.minLength.min }} символов</small>
                </span>
              </div>
            </div>
            <div class="col-md-8 mt-2" v-bind:class="{ 'fld-error': $v.register.email.$error }">
              <div class="md-form mb-0">  
                <label for="inputEmail" class="sr-only">"Электронная почта</label>
                <input id="inputEmail" class="form-control" placeholder="Электронная почта" v-model="register.email" @input="$v.register.email.$touch()">
                <span class="msg-error" v-if="!$v.register.email.required">
                  <small>Поле обязательно для заполнения</small>
                </span>
                <div class="msg-error" v-if="!$v.register.email.email">Неверно указана почта</div>
              </div>
            </div>
            <div class="col-md-8 mt-2" v-bind:class="{ 'fld-error': $v.register.firstname.$error }">
              <div class="md-form mb-0">  
                <label for="inputFirstName" class="sr-only">Имя</label>
                <input type="text" id="inputFirstName" class="form-control" placeholder="Имя" v-model="register.firstname" @input="$v.register.firstname.$touch()">
                <span class="msg-error" v-if="!$v.register.firstname.required">
                  <small>Поле обязательно для заполнения</small>
                </span>
              </div>
            </div>
            <div class="col-md-8 mt-2" v-bind:class="{ 'fld-error': $v.register.lastname.$error }">
              <div class="md-form mb-0">
                <label for="inputLastName" class="sr-only">Фамилия</label>
                <input type="text" id="inputLastName" class="form-control" placeholder="Фамилия" v-model="register.lastname" @input="$v.register.lastname.$touch()">
                <span class="msg-error" v-if="!$v.register.lastname.required">
                  <small>Поле обязательно для заполнения</small>
                </span>
              </div>
            </div> 
            <div class="col-md-8 mt-2" v-bind:class="{ 'fld-error': $v.register.password.$error }">
              <div class="md-form mb-0">
                <label for="inputPassword" class="sr-only">Пароль</label>
                <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль" v-model="register.password" @input="$v.register.password.$touch()">
                <span class="msg-error" v-if="!$v.register.password.required">
                  <small>Поле обязательно для заполнения</small>
                </span>
                <span class="msg-error" v-if="!$v.register.password.minLength">
                  <small>Пароль должен быть не меньше {{ $v.register.password.$params.minLength.min }} символов</small>
                </span>
              </div>
            </div>
            <div class="col-md-8 mt-2" v-bind:class="{ 'fld-error': $v.register.password2.$error }">
              <div class="md-form mb-0">
                <label for="inputPassword" class="sr-only">Пароль</label>
                <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль" v-model="register.password2" @input="$v.register.password2.$touch()">
                <span class="msg-error" v-if="!$v.register.password2.sameAsPassword">
                  <small>Пароли должены совпадать</small>
                </span>
              </div>
            </div>
            <button class="btn mt-4 btn-outline-success btn-block register-btn" type="submit" :disabled="$v.$invalid">Регистрация</button>
          </div>
        </form>
    </div>
  </div>
</template>

<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'
export default {
  layout: "post_detail",
  data() {
    return {
      register: {
        username: '',
        email: '',
        firstname: '',
        lastname: '',
        password: '',
        password2: '',
      },
    }
  },
  methods: {
    async userRegister() {
      try {
        let response = await this.$axios.post('/api/register/', {
          username: this.register.username,
          email: this.register.email,
          first_name: this.register.firstname,
          last_name: this.register.lastname,
          password: this.register.password,
          password2: this.register.password2
        })
        await this.$auth.loginWith('local', {
          data: {
            username: this.register.username,
            password: this.register.password
          },
        })
        this.$router.back()
      } catch (err) {
        console.log(err)
      }
    }
  },
   validations: {
  register: {
    username: {
      required,
      minLength: minLength( 3 )
    },
    email: {
      email,
      required
    },
    lastname: {
      required
    },
    firstname: {
      required
    },
    password: {
      required,
      minLength: minLength( 8 )
    },
    password2: {
      sameAsPassword: sameAs('password')
    }
  }
},
}
</script>

<style type="text/css">
.fld-error .msg-error  {
  display: block;
  color: #dc3545;
}
.msg-error {
  display: none;
}
</style>