<!-- RegisterPage.vue -->
<template>
  <div>
    <h1>Регистрация</h1>
    <!-- ... Ваш код для формы регистрации ... -->
    <button @click="register">Зарегистрироваться</button>
    <p @click="goToAuthPage">Уже есть аккаунт? Войдите</p>

  </div>
</template>

<script>
export default {
  methods: {
    register() {
      // Логика регистрации
      this.$emit('registerSuccess');
    },
    toggleLoginMode() {
      this.$emit('toggleLogin');
    },
    // Новый метод для перехода к окну авторизации
    goToAuthPage() {
      this.$emit('goToAuthPage');
    },
  },
};
</script>

<style scoped>
/* Ваши стили */
</style>
