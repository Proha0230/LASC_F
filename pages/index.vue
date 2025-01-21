<template>
  <div class="auth">
    <div class="auth-form">
      <div
          class="auth-form-label"
          v-text="'Авторизация'"
      />

      <a-form
          :model="resource"
          :rules="rules"
      >
        <a-form-item name="login">
          <AInput
              v-model:value="resource.login"
              placeholder="Логин"
              class="auth-form-input"
          />
        </a-form-item>

        <a-form-item name="password">
          <AInput
              v-model:value="resource.password"
              type="password"
              placeholder="Пароль"
              class="auth-form-input"
          />
          </a-form-item>
      </a-form>

      <div class="auth-form-button">
        <AButton
            class="auth-form-button-login"
            type="primary"
            @click.stop="auth"
        >Вход</AButton>

        <AButton
            @click.stop="registration"
        >Зарегистрироваться</AButton>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {Input as AInput, Button as AButton, Form as AForm} from "ant-design-vue"
import type { RuleObject } from "ant-design-vue/es/form"
const {Item: AFormItem} = AForm

const resource = ref({
  login: "",
  password: ""
})

const rules: Record<string, RuleObject[]> = {
    login: [{ required: true, message: "Логин пользователя обязателен", trigger: "blur", min: 4, max: 20 }],
    password: [{ required: true, message: "Пароль пользователя обязателен", trigger: "blur", min: 4, max: 20 }]
}

function auth() {
  if (resource.value.login && resource.value.password) {
    try {
      fetch("http://localhost:8080/auth", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(resource.value)
      })
          .then((response) => console.log(response))
          .catch((error) => console.log(error, "запрос не ушел"))
    } catch (e) {
      console.log(e, "ошибка запроса auth()")
    }
  }
}

function registration() {
  if (resource.value.login && resource.value.password) {
    try {
      fetch("http://localhost:8080/registration", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(resource.value)
      })
          .then((response) => console.log(response))
          .catch((error) => console.log(error, "запрос не ушел"))
    } catch (e) {
      console.log(e, "ошибка запроса registration()")
    }
  }
}
// https://lasc-frontend-default-rtdb.europe-west1.firebasedatabase.app

</script>

<style scoped lang="scss">
.auth-form {
  max-width: 50rem;
  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  border: solid 0.1rem #e4e4e4;
  border-radius: 0.5rem;
}

.auth-form-label {
  margin-top: 1rem;
}

.auth-form-input {
  width: 30rem;
  margin: 1rem;
}

.auth-form-button {
  display: flex;
  justify-content: center;
  width: 100%;
  padding-bottom: 1rem;
}

.auth-form-button-login {
  margin-right: 1rem;
}
</style>