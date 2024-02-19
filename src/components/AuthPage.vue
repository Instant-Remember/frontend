<template>
  <div class="AuthPage">
    <img src="/src/assets/img/backgroundAuth.svg" alt="" class="background">
    <!-- ... Ваш код для формы авторизации ... -->
    <section class="container">
      <section class="auth">
        <section class="authContainer">
          <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
          <p class="enter">Вход</p>
          <input type="email" class="email" placeholder="Почта">
          <input type="password" placeholder="Пароль">
          <button @click="login">Войти</button>
          
        </section>
        <p class="forgetPassword">Забыл пароль</p>
      </section>

      <section class="registration">
        <button @click="goToRegisterPage">Зарегистрироваться</button>
        <p>После регистрации вы получите доступ ко всем возможностям</p>
      </section>
    </section>
  </div>
</template>

<script>
export default {
  methods: {
    login() {
      // Логика авторизации
      this.$emit('loginSuccess');
    },
    toggleRegisterMode() {
      this.$emit('toggleRegister');
    },
    // Новый метод для перехода к странице регистрации
    goToRegisterPage() {
      this.$emit('goToRegisterPage');
    },
  },
};
</script>

<style scoped>
.AuthPage {
  display: flex;
}

.background {
  margin-top: 90px;
  margin-left: 64px;
}

.auth {
  width: 424px;
  height: 419px;
  background-color: #fff;
  border-radius: 20px;
  
  margin-top: 24px;
}

.authContainer{
  background-color: aqua;

  margin-top: 24px;
  margin-left: 56px;
}

.logo {
  width: 62px;
  height: 47px;
}

.registration {
  margin-top: 213px;
}
</style>
