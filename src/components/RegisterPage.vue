<template>
  <div class="RegisterPage">
    <img src="/src/assets/img/backgroundAuth.svg" alt="" class="background">
    <section class="container">

      <section class="registration">
        <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
        <p class="registr">Регистрация</p>
        <component :is="currentStepComponent" @nextStep="nextStep" @registerSuccess="registerSuccess" />
      </section>

      

      <section class="enter">
      <button @click="goToAuthPage">Войти</button>
      <p>После входа вы получите доступ ко всем возможностям</p>
    </section>
    </section>
  </div>
</template>

<script>
import RegisterStep1 from './RegisterStep1.vue';
import RegisterStep2 from './RegisterStep2.vue';
import RegisterStep3 from './RegisterStep3.vue';

export default {
  components: { RegisterStep1, RegisterStep2, RegisterStep3 },

  data() {
    return {
      currentStep: 1,
    };
  },
  computed: {
    currentStepComponent() {
      switch (this.currentStep) {
        case 1:
          return 'RegisterStep1';
        case 2:
          return 'RegisterStep2';
        case 3:
          return 'RegisterStep3';
        default:
          return 'RegisterStep1';
      }
    },
  },
  methods: {
    nextStep(data) {
      this.currentStep++;
      // Возможно, здесь будет нужна логика сохранения данных между этапами
    },
    registerSuccess() {
      // Логика после успешной регистрации (например, переход на главную страницу)
      this.$emit('registerSuccess');

    },
    register() {
      // Логика регистрации
      this.$emit('registerSuccess');
    },
    goToAuthPage() {
      this.$emit('goToAuthPage');
    },
  },
};
</script>

<style scoped>
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
.registration {
  width: 424px;
  height: 419px;
  background-color: #fff;
  border-radius: 20px;

  padding-top: 24px;
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
</style>
