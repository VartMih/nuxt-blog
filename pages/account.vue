<template>
  <div class="register-top"> 
    <div class="card text-white bg-dark mb-3 update-card" style="max-width: 38rem;">
      <div class="card-header card-header-edit">Ваши данные:</div>
          <div class="mt-2 user-update">
            <div class="datauser-block">
              <h3 class="mb-0">{{datauser.username}}</h3>
              <button class="btn-edit" v-on:click="isHiddenUsername = !isHiddenUsername">Редактировать</button>
            </div>
            <form @submit.prevent="userUpdate">
              <div v-if="!isHiddenUsername" class="col-md-10" v-bind:class="{ 'fld-error': $v.register.username.$error }">
                <div class="md-form mb-0 form-update">
                  <label for="inputUsername" class="sr-only">Имя пользователя</label>
                  <input type="text" id="inputUsername" class="form-control user-update-form" v-model="register.username" @input="$v.register.username.$touch()">
                  <button class="btn mt-0 ml-2 btn-outline-light btn-sm" type="submit">Сохранить</button>
                </div>
                <span class="msg-error" v-if="!$v.register.username.minLength">
                    <small>Должно быть не меньше {{ $v.register.username.$params.minLength.min }} символов</small>
                </span>
              </div>
            </form>
          </div>
          <div class="mt-2 user-update">
            <div class="datauser-block">
              <h3 class="mb-0">{{datauser.first_name}}</h3>
              <button class="btn-edit" v-on:click="isHiddenFirst_name = !isHiddenFirst_name">Редактировать</button>
            </div>
            <form @submit.prevent="userUpdate">
              <div v-if="!isHiddenFirst_name  " class="col-md-10" v-bind:class="{ 'fld-error': $v.register.first_name.$error }">
                <div class="md-form mb-0 form-update">
                  <label for="inputUsername" class="sr-only">Email</label>
                  <input type="text" id="inputUsername" class="form-control user-update-form" v-model="register.first_name" @input="$v.register.first_name.$touch()">
                  <button class="btn mt-0 ml-2 btn-outline-light btn-sm" type="submit">Сохранить</button>
                </div>
              </div>
            </form>
          </div><div class="mt-2 user-update">
            <div class="datauser-block">
              <h3 class="mb-0">{{datauser.last_name}}</h3>
              <button class="btn-edit" v-on:click="isHiddenLast_name = !isHiddenLast_name">Редактировать</button>
            </div>
            <form @submit.prevent="userUpdate">
              <div v-if="!isHiddenLast_name" class="col-md-10" v-bind:class="{ 'fld-error': $v.register.last_name.$error }">
                <div class="md-form mb-0 form-update">
                  <label for="inputUsername" class="sr-only">Email</label>
                  <input type="text" id="inputUsername" class="form-control user-update-form" v-model="register.last_name" @input="$v.register.last_name.$touch()">
                  <button class="btn mt-0 ml-2 btn-outline-light btn-sm" type="submit">Сохранить</button>
                </div>
              </div>
            </form>
          </div>
          <div class="mt-2 user-update">
            <div class="datauser-block">
              <h3 class="mb-0">{{datauser.email}}</h3>
              <button class="btn-edit" v-on:click="isHiddenEmail = !isHiddenEmail">Редактировать</button>
            </div>
            <form @submit.prevent="userUpdate">
              <div v-if="!isHiddenEmail" class="col-md-10" v-bind:class="{ 'fld-error': $v.register.email.$error }">
                <div class="md-form mb-0 form-update">
                  <label for="inputUsername" class="sr-only">Email</label>
                  <input type="text" id="inputUsername" class="form-control user-update-form" v-model="register.email" @input="$v.register.email.$touch()">
                  <button class="btn mt-0 ml-2 btn-outline-light btn-sm" type="submit">Сохранить</button>
                </div>
                <span class="msg-error" v-if="!$v.register.email.email">
                  <small>Неверный электронный адрес</small>
                </span>
              </div>
            </form>
          </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators';
export default {
    layout: "post_detail",
  data: function () {
    return {
       isHiddenUsername: true,
       isHiddenFirst_name: true,
       isHiddenLast_name: true,
       isHiddenEmail: true,
      datauser: [],
      register: {
        username: '',
        email: '',
        first_name: '',
        last_name: '',
        password: '',
        password2: '',
      },
    }
  },
  methods: {
    async userUpdate() {
        let id = this.datauser.id;
        let response = await this.$axios.$put(`http://127.0.0.1:8000/api/profile/update/${id}`, {
          username: this.register.username.length > 2 ? this.register.username : this.datauser.username,
          email: this.register.email.length > 1 ? this.register.email : this.datauser.email,
          first_name: this.register.first_name.length > 1 ? this.register.first_name : this.datauser.first_name,
          last_name: this.register.last_name.length > 1 ? this.register.last_name : this.datauser.last_name,
          password: this.register.password.length > 1 ? this.register.password : this.datauser.password,
          date_joined: this.datauser.date_joined
        })
        this.$router.go(0)
    }
  },
  mounted() {
    this.$axios.get(`http://127.0.0.1:8000/api/profile`, { crossdomain: true }).then((result) => {
      this.datauser = result.data.user
    });
  },
  status(validation) {
        return {
        error: validation.$error,
        dirty: validation.$dirty
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
    last_name: {
      required
    },
    first_name: {
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
}
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