<template>
  <div class="friendsMenu">
    <div class="friend1" v-for="(subscription, index) in subscriptions" :key="index">
      <img src="/src/assets/img/friends/andreyK.svg" alt="">
      <text>Андрей</text>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      subscriptions: []
    };
  },

  created() {
    this.fetchUserSubscriptions()
  },

  methods: {
    fetchUserSubscriptions() {
      const accessToken = localStorage.getItem('accessToken');

      axios.get('http://51.250.111.113:8000/me/subscriptions', {
        headers: {
          Authorization: `Bearer ${accessToken}`
        }
      })
        .then(response => {
          console.log('Подписки:', response.data);
          this.subscriptions = response.data; // Заполнение массива целей из ответа сервера
        })
        .catch(error => {
          console.error('Ошибка при получении подписок:', error);
        });
    },
  }
}
</script>

<style scoped>
.friendsMenu {
  width: 202px;
  background-color: #fff9f9;
  border-radius: 20px;

  padding-left: 31px;

  justify-content: center;

  display: flex;
  flex-direction: column;

  
  padding-bottom: 15px;
}

.friend1 {
  display: flex;
  align-items: center;
  margin-top: 20px;
  height: 36px;

  cursor: pointer;
}

img {
  width: 36px;
  height: 36px;
}

text {
  margin-left: 24px;
}
</style>
