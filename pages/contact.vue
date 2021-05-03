<template>
  <div>
    <Header_contact :title="title"/>

    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <nav aria-label="breadcrumb" class="mt-4">
            <ol class="breadcrumb">
              <li class="breadcrumb-item"><a href="">Главная</a></li>
              <li class="breadcrumb-item active" aria-current="page">Контакты</li>
            </ol>
          </nav>
          <p class="lead">Форма для обратной связи</p>
          <small class="text-muted">Для успешной отправки формы заполните все поля</small>
          <form name="contact-form">
            <div class="row mt-3">
              <div class="col-md-6" v-bind:class="{ 'fld-error': $v.form.name.$error }">
                <div class="md-form mb-0">
                  <label for="name" class="sr-only">Ваше имя</label>
                  <input type="text" id="name" class="form-control" placeholder="Ваше имя" v-model="form.name" @input="$v.form.name.$touch()">
                </div>
                <span class="msg-error" v-if="!$v.form.name.required">
                  <small>Поле обязательно для заполнения</small>
                </span>
                <span class="msg-error" v-if="!$v.form.name.minLength">
                  <small>Должно быть не меньше {{ $v.form.name.$params.minLength.min }} символов.</small>
                </span>
              </div>

              <div class="col-md-6" v-bind:class="{ 'fld-error': $v.form.email.$error }">
                <div class="md-form mb-0">
                  <label for="email" class="sr-only">Ваша почта</label>
                  <input type="text" id="email" class="form-control" placeholder="Ваша почта" v-model="form.email"
                  @input="$v.form.email.$touch()">
                </div>
                <span class="msg-error" v-if="!$v.form.email.required">
                  <small>Поле обязательно для заполнения</small>
                </span>
                <span class="msg-error" v-if="!$v.form.email.minLength">
                  <small>Невалидный email</small>
                </span>
              </div>
            </div>

            <div class="row mt-3" v-bind:class="{ 'fld-error': $v.form.phone.$error }">
              <div class="col-md-12">
                <div class="md-form mb-0">
                  <label for="subject" class="sr-only">Ваш телефон</label>
                  <input type="text" id="subject" class="form-control" placeholder="Ваш телефон" v-model="form.phone"
                  @input="$v.form.phone.$touch()">
                </div>
                <span class="msg-error" v-if="!$v.form.phone.required">
                  <small>Номер телефона в формате 89009009090</small>
                </span>
                <span class="msg-error" v-if="!$v.form.phone.minLength">
                  <small>Невалидный номер</small>
                </span>
              </div>
            </div>

            <div class="row mt-3">
              <div class="col-md-12" v-bind:class="{ 'fld-error': $v.form.subject.$error }">
                <div class="md-form mb-0">
                  <label for="subject" class="sr-only">Тема</label>
                  <input type="text" id="subject" class="form-control" placeholder="Тема" v-model="form.subject" @input="$v.form.subject.$touch()">
                </div>
                <span class="msg-error" v-if="!$v.form.subject.required">
                  <small>Поле обязательно для заполнения</small>
                </span>
                <span class="msg-error" v-if="!$v.form.subject.minLength">
                  <small>Должно быть не меньше {{ $v.form.subject.$params.minLength.min }} символов.</small>
                </span>
              </div>
            </div>

            <div class="row mt-3">
              <div class="col-md-12">
                <div class="md-form">
                  <label for="message" class="sr-only">Ваше сообщение</label>
                  <textarea type="text" id="message" rows="2" class="form-control md-textarea" placeholder="Ваше сообщение" v-model="form.message"></textarea>
                </div>
              </div>
            </div>
          </form>
          <div class="text-center text-md-left mt-3">
            <button class="btn btn-primary" type="submit" @click.prevent="submitForm" :disabled='!isComplete'>Отправить</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Header_contact from "@/components/Header_contact";
import { required, minLength, email, numeric } from 'vuelidate/lib/validators'

export default {
  components: {Header_contact},
  layout: "w_p_detail",
  data() {
    return {
      title: "Свяжитесь с нами через указанные контакты или заполните форму ниже.",
      form: {
        name: '',
        email: '',
        phone: '',
        subject: '',
        message: '',
      }
    }
  },
  methods: {
    submitForm() {
      let contactFormData = new FormData();
      contactFormData.set('name', this.form.name);
      contactFormData.set('email', this.form.email);
      contactFormData.set('phone', this.form.phone);
      contactFormData.set('subject', this.form.subject);
      contactFormData.set('message', this.form.message);
      console.log('submitting data...');
      axios({
        method: 'post',
        url: 'http://localhost:8000/api/feedback/',
        data: contactFormData
      }).then(function (response) {
        console.log(response);
      }).catch(function (response) {
        console.log(response);
      });
      this.$router.push("/success");
    }
  },
  computed: {
      isComplete () {
        return this.form.name && this.form.email && this.form.phone && this.form.subject && this.form.message;
      }
    },
  validations: {
  form: {
    name: {
      required,
      minLength: minLength( 3 )
    },
    email: {
      email,
      required
    },
    phone: {
      numeric,
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
