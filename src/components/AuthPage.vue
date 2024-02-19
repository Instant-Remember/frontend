<template>
  <div class="AuthPage">
    <img src="/src/assets/img/backgroundAuth.svg" alt="" class="background">

    <section class="container">
      <section class="auth">
        <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
        <p class="enter">Вход</p>
        <input type="email" class="email" placeholder="Почта">
        <input type="password" placeholder="Пароль" class="password">
        <button @click="login" class="login">Войти</button>
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

.container {
  padding-top: 24px;
}

.auth {
  width: 424px;
  height: 419px;
  background-color: #fff;
  border-radius: 20px;

  padding-top: 24px;
}

.logo {
  width: 62px;
  height: 47px;

  margin-left: 181px;
}

.enter {
  width: 58px;
  height: 29px;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 24px;
  line-height: 29px;

  display: flex;
  align-items: center;

  color: #444444;

  margin-left: 183px;
}

input {
  background: #E8ECF2;
  border-radius: 20px;

  width: 312px;
  height: 50px;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;

  display: flex;
  align-items: center;

  margin-left: 56px;

}

input::placeholder {
    text-indent: 20px;
}

.email {
  margin-top: 24px;
}

.password {
  margin-top: 24px;
}

button {
  width: 312px;
  height: 49px;

  margin-left: 56px;
  margin-top: 24px;

  border-radius: 20px;

  border: none;
  outline: none;

  color: #fff;
  background-color: #6133B4;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;

  cursor: pointer;
}

.forgetPassword {
  margin-top: 24px;
  margin-left: 161px;


  width: 104px;
  height: 18px;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 15px;
  line-height: 18px;
  display: flex;
  align-items: center;

  color: #8C8C8C;


}

.registration {
  width: 424px;
  height: 157px;

  border-radius: 20px;
  background-color: #fff;
  margin-top: 24px;
}

.registration p {

  width: 307px;
  height: 36px;

  margin-left: 58px;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 15px;
  line-height: 18px;
  display: flex;
  align-items: center;
  text-align: center;

  color: #8C8C8C;
}
</style>
