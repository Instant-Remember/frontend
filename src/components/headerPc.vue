<template>
    <div class="headerPc">
        <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
        <searchLine></searchLine>
        <div class="menu_buttons">
            <p id="p1">Популярное</p>
            <p id="p2">Для тебя</p>
        </div>
        <div class="user">
            <img src="/src/assets/img/avatar.svg" alt="Me">
            <p>{{ user.first_name }} {{ user.last_name }}</p>
        </div>
    </div>
</template>
  
<script>
import searchLine from './searchLine.vue'
import axios from 'axios';

export default {
  components: {searchLine},

  data() {
    return {
      user: {}
    };
  },
  created() {
    this.fetchUserData();
  },
  methods: {
    fetchUserData() {
      // Получение токена доступа из локального хранилища
      const accessToken = localStorage.getItem('accessToken');

      // Выполнение запроса к серверу с токеном доступа
      axios.get('http://158.160.108.155:8000/me', {
        headers: {
          Authorization: `Bearer ${accessToken}`
        }
      })
      .then(response => {
        // Обработка успешного ответа
        console.log('Информация о пользователе:', response.data);
        this.user = response.data;
      })
      .catch(error => {
        // Обработка ошибки
        console.error('Ошибка:', error);
        this.error = error.message;
      });
    }
  }
}
</script>

<style scoped>
* {
    display: flex;
}

.headerPc {
    width: 1440px;
    height: 70px;
    background-color: #fff9f9;
    display: flex;

    margin-left: auto;
    margin-right: auto;


    align-items: center;

    width: 1440px;

}

.logo {

    margin-left: 64px;

    cursor: pointer;
}

.searchLine {
    width: 332px;
    margin-left: 272px;
}

.menu_buttons {
    margin-left: 88px;

    width: 250px;
    height: 19px;
    display: flex;
}

p {
    height: 19;
    margin-top: 0;


    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;

    color: #444444;

    cursor: pointer;

    white-space: nowrap;

}

#p2 {
    padding-left: 84px;
}

.user {
    width: 184px;
    height: 42px;

    margin-left: 124px;

    cursor: pointer;
}

.user img {
    width: 42px;
    height: 42px;
}

.user p {
    margin-top: 12px;
    margin-left: 24px;
    height: 19px;
    font-size: 16px;
}
</style>