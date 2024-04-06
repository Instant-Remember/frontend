<template>
  <div class="goalsPage">
    <profileHeader></profileHeader>

    <search>
      <searchLine></searchLine>
      <button @click="showPopup = true">Создать</button>
    </search>

    <popup v-if="showPopup" @close="showPopup = false">
      <!-- Содержимое всплывающего окна передается через слот -->
      <goalCreate>
        <input type="text" v-model="goalName" placeholder="Название цели" class="goalName">
        <input type="text" v-model="goalDescription" placeholder="Описание..." class="goalDescription">

        <button @click="createGoal">Создать</button>
      </goalCreate>

    </popup>

    <section>
      <goal></goal>
    </section>
  </div>
</template>

<script>
import searchLine from './searchLine.vue'
import goal from './goal.vue'
import Popup from './Popup.vue'
import profileHeader from './profileHeader.vue'
import progressBar from './progressBar.vue'

import axios from 'axios';

export default {
  components: { searchLine, goal, Popup, profileHeader, progressBar },

  data() {
    return {
      showPopup: false,
      user: {},
      goalName: '',
      goalDescription: ''
    };
  },
  
  methods: {
    fetchUserData() {
      // Получение токена доступа из локального хранилища
      const accessToken = localStorage.getItem('accessToken');

      // Выполнение запроса к серверу с токеном доступа
      axios.get('http://51.250.79.67:8000/me', {
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
    },

    createGoal() {
      const goalData = {
        name: this.goalName,
        description: this.goalDescription,
        owner_id: this.user.id,
        date_create: new Date().toISOString(),
        date_modify: new Date().toISOString()
      };

      // Получение токена доступа из локального хранилища
      const accessToken = localStorage.getItem('accessToken');

      // Выполнение запроса на создание цели
      axios.post('http://51.250.79.67:8000/goal/', goalData, {
        headers: {
          Authorization: `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      })
      .then(response => {
        // Обработка успешного ответа
        console.log('Цель успешно создана:', response.data);
        // Дополнительные действия при необходимости, например, обновление списка целей
        // или закрытие всплывающего окна
        this.showPopup = false;
      })
      .catch(error => {
        // Обработка ошибки
        console.error('Ошибка при создании цели:', error);
      });
    }
  },

  mounted() {
    // Выполнение запроса на получение информации о пользователе при загрузке компонента
    this.fetchUserData();
  }
}
</script>


<style scoped>
.goalsPage {
  width: 757px;
}

search {

  margin-left: 397px;
  margin-top: 16px;

  width: 756px;
  height: 52px;

  background-color: #fff;

  display: flex;
  align-items: center;

  border-radius: 20px;
}

search button {
  width: 135px;
  height: 41px;

  margin-left: 16px;

  border-radius: 14px;

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

.searchLine {
  margin-left: 16px;

  width: 573px;
}

section {
  width: 757px;

  background-color: #fff;
  border-radius: 20px;

  padding-top: 1px;
  padding-bottom: 16px;

  margin-top: 16px;
  margin-left: 397px;
}

.goalName {

width: 609px;
height: 36px;
margin-top: 15px;
margin-left: 36px;
}

.goalDescription {

width: 609px;
height: 118px;
margin-left: 36px;

}

goalCreate button {
margin-top: 12px;
margin-left: 220px;

width: 246px;
height: 49px;

background: #6133B4;
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