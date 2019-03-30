<template lang="pug">
  .login
    .login__content
      button(type="button").login__form-close
      form.login__form(@submit.prevent="login")
        h1.login__form-title Авторизация
        .login__row
          app-input(
            title="Логин"
            icon="user"
            v-model="user.name"
            :errorText="validation.firstError('user.name')"
          )
        .login__row
          app-input(
            title="Пароль"
            icon="key"
            type="password"
            v-model="user.password"
            :errorText="validation.firstError('user.password')"
          )
        .login__btn
          button(
            type="submit"
            :disabled="disableSubmit"
          ).login__send-data Отправить
</template>

<script>
import { Validator } from "simple-vue-validator";
import axios from "axios";

export default {
  mixins: [require("simple-vue-validator").mixin],
  validators: {
    "user.name": value => {
      return Validator.value(value).required("Введите имя пользователя");
    },
    "user.password": value => {
      return Validator.value(value).required("Введите пароль");
    }
  },
  data() {
    return {
      disableSubmit: false,
      user: {
        name: "",
        password: ""
      }
    };
  },
  components: {
    appInput: () => import("./components/input")
  },
  methods: {
    async login() {
      if ((await this.$validate()) === false) return;
      this.disableSubmit = true;

      try {
        axios
          .post("//jsonplaceholder.typicode.com/posts", {
            name: this.user.name,
            password: this.user.password
          })
          .then(response => {
            const report = JSON.stringify(response, null, 2);
            console.log(report);
          });
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="pcss">
@import "normalize.css";
@import "../styles/mixins.pcss";
@import "../styles/layout/base.pcss";
@import url("https://fonts.googleapis.com/css?family=Open+Sans:300,400,600,700,800");

.root-wrapper-container {
  height: 100%;
}

.header-container {
  background: linear-gradient(to right, #3e3e59, #454573);
  padding: 15px 0;

  @include phones {
    padding: 20px 0;
  }
}

.root-container {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.admin-wrapper {
  display: flex;

  .maincontent {
    width: 100%;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }
}

.page-title {
  margin-bottom: 60px;
  font-size: 21px;
  font-weight: bold;
}

.tooltips-container {
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translate(-50%, 110%);
  visibility: hidden;
  transition: 0.3s;

  &.showed {
    transform: translate(-50%, 0%);
    visibility: visible;
  }
}

button {
  background: transparent;
}

/* Форма логина */
@import "../../../styles/mixins.pcss";
.login {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: url("~images/bg/admin.jpg") center center / cover no-repeat;

  &:before {
    content: "";
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    opacity: 0.5;
    background: $text-color;
  }
}

.login__form-title {
  font-size: 36px;
  text-align: center;
  font-weight: 600;
}

.login__content {
  position: relative;

  @include phones {
    height: 100%;
    width: 100%;
  }
}

.login__row {
  margin-bottom: 35px;
}

.login__btn {
  display: flex;
  width: 100%;
  padding: 0 8%;
  justify-content: center;
}

.login__send-data {
  width: 100%;
  padding: 30px;
  background-image: linear-gradient(to right, #ad00ed, #5500f2);
  border-radius: 40px 0 40px;
  color: #fff;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 18px;

  &[disabled] {
    opacity: 0.5;
    filter: grayscale(100%);
  }
}

.login__form {
  width: 563px;
  padding: 50px 77px 60px;
  background: #fff;

  @include phones {
    width: 100%;
    padding-right: 7%;
    padding-left: 7%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
}
</style>
