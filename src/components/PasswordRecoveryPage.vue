<template>
  <div class="PasswordRecoveryPage">
    <img src="/src/assets/img/backgroundAuth.svg" alt="" class="background">

    <section class="container">
      <section class="recovery">
        <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
        <p class="recoveryText">Восстановление пароля</p>
        <input type="email" class="email" placeholder="Почта">
        <button @click="goToAuthPage" class="recover">Восстановить</button>
        <!--<p class="backToLogin" @click="recoverPassword">Вернуться к входу</p>-->
      </section>
      <section class="enter">
        <button @click="goToAuthPage">Войти</button>
        <p>После входа вы получите доступ ко всем возможностям</p>
      </section>
    </section>
  </div>
</template>

<script>
export default {
  props: {
        backendURL: String
    },

  methods: {
    recoverPassword() {
      // Логика восстановления пароля
      this.$emit('recoverPasswordSuccess');
    },
    goToAuthPage() {
      this.$emit('goToAuthPage');
    },
  },
};
</script>

<style scoped>
.PasswordRecoveryPage {
  display: flex;
}

.background {
  margin-top: 90px;
  margin-left: 64px;
}

.recovery {
  width: 424px;
  background-color: #fff;
  border-radius: 20px;
  padding-top: 24px;
  padding-bottom: 57px;

  margin-top: 56px;
}

.logo {
  width: 62px;
  height: 47px;
  margin-left: 181px;
}

.recoveryText {
  width: 286px;
  height: 29px;
  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 24px;
  line-height: 29px;
  display: flex;
  align-items: center;
  color: #444444;
  margin-left: 70px;
}

input {
  background: #E8ECF2;
  border-radius: 20px;
  width: 292px;
  height: 50px;
  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;
  display: flex;
  align-items: center;
  margin-left: 56px;
  margin-top: 83px;
}

input::placeholder {
  text-indent: 20px;
}

.recover {
  width: 312px;
  height: 49px;
  margin-left: 56px;
  margin-top: 56px;
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


.enter {
  width: 424px;
  height: 157px;

  border-radius: 20px;
  background-color: #fff;
  margin-top: 24px;
}

.enter p {

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

@media (max-width: 425px) {
  .PasswordRecoveryPage {
    width: 375px;
  }

  .background {
    display: none;
  }

  .container {
    margin-left: 16px;
    margin-top: 96px;
  }

  input {
    width: 292px;
    height: 50px;
    margin-left: 17px;
  }

  .logo {
    margin-top: 24px;
    margin-left: 140px;
  }

  .recover{
    margin-left: 17px;
    margin-top: 24px;
  }

  button {
    margin-left: 17px;
    margin-top: 24px;
  }

  .enter {
    width: 343px;
  }

  .enter p {
    margin-left: 18px;
  }
}
</style>