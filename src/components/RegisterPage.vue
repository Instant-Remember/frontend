<template>
  <div class="RegisterPage">
    <img src="/src/assets/img/backgroundAuth.svg" alt="" class="background">

    <section class="container">
      <div class="steps">
        <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
        <p class="registr">Регистрация</p>

        <div v-if="step === 1" class="step1">
          <input type="text" placeholder="Имя" id="first_name" v-model="userData.first_name" required>
          <input type="text" placeholder="Фамилия" id="last_name" v-model="userData.last_name" required>
          <div class="circles">
            <div class="circle" :class="{ 'active': activeCircle === 1 }"></div>
            <div class="circle"></div>
            <div class="circle"></div>
          </div>
          <button @click="nextStep">Далее</button>
        </div>
        <div v-else-if="step === 2" class="step2">
          <input type="text" placeholder="Имя пользователя" id="username" v-model="userData.username" required>
          <input type="email" placeholder="Почта" id="email" v-model="userData.email" required>
          <div class="circles">
            <div class="circle active"></div>
            <div class="circle active"></div>
            <div class="circle"></div>
          </div>
          <button @click="nextStep">Далее</button>
        </div>
        <div v-else class="step3">
          <input type="password" placeholder="Пароль" id="password" v-model="userData.password" required>
          <input type="password" v-model="repeatPassword" placeholder="Повторите пароль">
          <div class="circles">
            <div class="circle active"></div>
            <div class="circle active"></div>
            <div class="circle active"></div>
          </div>

          <button @click="signup">Зарегистрироваться</button>
        </div>
      </div>

      <section class="enter">
        <button @click="goToAuthPage">Войти</button>
        <p>После входа вы получите доступ ко всем возможностям</p>
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
      step: 1,
      userData: {
        username: '',
        email: '',
        first_name: '',
        last_name: '',
        password: '',
      }
    };
  },

  methods: {
    nextStep() {
      this.step++;
      this.activeCircle++;
    },
    goToAuthPage() {
      this.$emit('goToAuthPage');
    },
    signup() {
      axios.post(`${this.backendURL}/signup`, this.userData)
        .then(response => {
          // Обработка успешного ответа от сервера
          console.log('Успешная регистрация:', response.data);
          this.$emit('goToAuthPage');
        })
        .catch(error => {
          // Обработка ошибки
          console.error('Ошибка при регистрации:', error);
        });
    },
  }
}
</script>

<style scoped>
.circle {
  width: 20px;
  height: 20px;
  border-radius: 4px;
  background-color: #E0E6EF;
  display: inline-block;
  margin: 0 5px;

  width: 95px;
  height: 8px;
}

.circle.active {
  background-color: #6133B4;
  width: 95px;
  height: 8px;
  border-radius: 4px;
}

.circles {
  display: flex;
  width: 312px;
  height: 8px;

  margin-top: 24px;
  margin-left: 62px;
}

.RegisterPage {
  display: flex;
}


.background {
  margin-top: 90px;
  margin-left: 64px;
}


.container {
  padding-top: 24px;
}


.steps {
  width: 424px;
  height: 419px;
  background-color: #fff;
  border-radius: 20px;
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
  margin-top: 24px;

}





.logo {
  width: 62px;
  height: 47px;


  margin-left: 170px;
}


.registr {
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


  margin-left: 136px;
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

@media (max-width: 375px) {
  .RegisterPage {
    width: 375px;
  }

  .background {
    display: none;
  }

  .container {
    margin-left: 16px;
    margin-top: 96px;
  }

  .steps {
    width: 343px;
    height: 419px;
    background-color: #fff;
    border-radius: 20px;
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

  .registr {
    margin-left: 100px;
  }

  button {
    margin-left: 17px;
    margin-top: 24px;
  }

  .circles {
    display: flex;
    width: 312px;
    height: 8px;

    margin-top: 24px;
    margin-left: 15px;
  }

  .enter {
    width: 343px;
  }


  .enter p {
    margin-left: 18px;
  }
}
</style>