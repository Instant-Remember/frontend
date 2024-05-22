<template>
  <div class="AuthPage">
    <img src="/src/assets/img/backgroundAuth.svg" alt="" class="background">

    <section class="container">
      <section class="auth">
        <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
        <p class="enter">Вход</p>
        <input type="text" placeholder="Почта" id="username" v-model="username" required>
        <input type="password" placeholder="Пароль" id="password" v-model="password" required>
        <button @click="login" class="login">Войти</button>
        <p class="forgetPassword" @click="openPasswordRecovery">Забыл пароль</p>
      </section>

      <section class="registration">
        <button @click="goToRegisterPage">Зарегистрироваться</button>
        <p>После регистрации вы получите доступ ко всем возможностям</p>
      </section>
    </section>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    backendURL: String
  },

  data() {
    return {
      username: '',
      password: ''
    };
  },
  methods: {
    // Новый метод для перехода к странице регистрации
    goToRegisterPage() {
      this.$emit('goToRegisterPage');
    },
    openPasswordRecovery() {
      this.$emit('openPasswordRecovery');
    },
    login() {
      const backendURL = localStorage.getItem('backendURL')
      axios.post(`http://158.160.92.119:8000/login`, {
        grant_type: '',
        username: this.username,
        password: this.password,
        scope: '',
        client_id: '',
        client_secret: ''
      }, {
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/x-www-form-urlencoded'
        }
      })
        .then(response => {
          // Обработка успешного ответа
          console.log('Успешный ответ от сервера:', response.data);
          localStorage.setItem('accessToken', response.data.access_token);

          this.$emit('loginSuccess');
        })
        .catch(error => {
          // Обработка ошибки
          console.error('Ошибка:', error);
        });
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
  margin-top: 20px;

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

  cursor: pointer;
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

@media (max-width: 375px) {
  .AuthPage {
    width: 375px;
  }

  .background {
   display: none;
  }

  .container{
    margin-left: 16px;
    margin-top: 96px;
  }

  .auth {
    width: 343px;
    height: 419px;
    background-color: #fff;
    border-radius: 20px;
  }

  .logo {
    width: 62px;
    height: 47px;

    margin-left: 148px;
  }

  .enter {
    margin-left: 150px;
  }

  input {
    margin-left: 16px;
  }

  button {
    margin-left: 17px;
    margin-top: 24px;
  }

  .forgetPassword {
    margin-left: 122px;
  }

  .registration {
    width: 343px;
  }

  .registration p {
    margin-left: 18px;
  }

}
</style>
