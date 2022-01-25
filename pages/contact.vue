<template>
  <div>
    <Header :h1="title" />
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <nav aria-label="breadcrumb" class="mt-4">
            <ol class="breadcrumb">
              <li class="breadcrumb-item"><a href="/">Главная</a></li>
              <li class="breadcrumb-item active" aria-current="page">
                Контакты
              </li>
            </ol>
          </nav>
          <p class="lead">
            Чтобы связаться со мной заполните форму обратной связи
          </p>
          <form name="contact-form">
            <div class="row">
              <div class="col-md-6" v-bind:class="{ 'fld-error': $v.form.name.$error }">
                <div class="md-form mb-0">
                  <label for="name" class="sr-only">Ваше имя</label>
                  <input type="text" id="name" class="form-control mt-2 feedback" placeholder="Ваше имя" v-model="form.name" @input="$v.form.name.$touch()"/>
                  <span class="msg-error" v-if="!$v.form.name.required">
                    <small>Поле обязательно для заполнения</small>
                  </span>
                  <span class="msg-error" v-if="!$v.form.name.minLength">
                    <small>Должно быть не меньше {{ $v.form.name.$params.minLength.min }} символов</small>
                  </span>
                </div>
              </div>

              <div class="col-md-6" v-bind:class="{ 'fld-error': $v.form.email.$error }">
                <div class="md-form mb-0">
                  <label for="email" class="sr-only">Ваша почта</label>
                  <input type="text" id="email" class="form-control mt-2 feedback" placeholder="Ваша почта" v-model="$v.form.email.$model" :class="status($v.form.email)"/>
                  <span class="msg-error" v-if="!$v.form.email.required">
                    <small>Поле обязательно для заполнения</small>
                  </span>
                  <div class="msg-error" v-if="!$v.form.email.email">Неверно указана почта</div>
                </div>
              </div>
            </div>

            <div class="row mt-3" v-bind:class="{ 'fld-error': $v.form.subject.$error }">
              <div class="col-md-12">
                <div class="md-form mb-0">
                  <label for="subject" class="sr-only">Тема</label>
                  <input type="text" id="subject" class="form-control feedback" placeholder="Тема" v-model="form.subject" @input="$v.form.subject.$touch()"/>
                   <span class="msg-error" v-if="!$v.form.subject.required">
                    <small>Поле обязательно для заполнения</small>
                  </span>
                  <span class="msg-error" v-if="!$v.form.subject.minLength">
                    <small>Должно быть не меньше {{ $v.form.subject.$params.minLength.min }} символов</small>
                  </span>
                </div>
              </div>
            </div>

            <div class="row mt-3" v-bind:class="{ 'fld-error': $v.form.message.$error }">
              <div class="col-md-12">
                <div class="md-form">
                  <label for="message" class="sr-only">Ваше сообщение</label>
                  <textarea type="text" id="message" rows="2" class="form-control md-textarea" placeholder="Ваше сообщение" v-model="form.message" @input="$v.form.message.$touch()"></textarea>
                   <span class="msg-error" v-if="!$v.form.message.required">
                    <small>Поле обязательно для заполнения</small>
                  </span>
                </div>
              </div>
            </div>
          </form>
          <div class="text-center text-md-left mt-3">
            <button class="btn btn-outline-dark" type="submit" @click.prevent="submitForm" :disabled="$v.$invalid">Отправить</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Header from "@/components/Header";
import { required, minLength, email } from 'vuelidate/lib/validators'
export default {
  components: { Header },
  layout: "post_detail",
  data() {
    return {
      title: "Написать мне",
      form: {
        name: "",
        email: "",
        subject: "",
        message: "",
      },
    };
  },
  methods: {
    submitForm() {
      let contactFormData = new FormData();
      contactFormData.set("name", this.form.name);
      contactFormData.set("email", this.form.email);
      contactFormData.set("subject", this.form.subject);
      contactFormData.set("message", this.form.message);
      axios({
        method: "post",
        url: "http://localhost:8000/api/feedback/",
        data: contactFormData,
      })
        .then(function (response) {
          console.log(response);
        })
        .catch(function (response) {
          console.log(response);
        });
      this.$router.push("/success");
    },
    status(validation) {
        return {
        error: validation.$error,
        dirty: validation.$dirty
      }
    },
  },

  validations: {
  form: {
    name: {
      required,
      minLength: minLength( 4 )
    },
    email: {
      email,
      required
    },
    subject: {
      required,
      minLength: minLength( 10 )
    },
    message: {
      required,
    }
  }
},
};
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