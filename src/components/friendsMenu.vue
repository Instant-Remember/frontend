<template>
  <div class="friendsMenu">
    <div class="friend1" v-for="(subscription, index) in subscriptions" :key="index"
      @click="showFriendName(subscription)">
      <img
        :src="subscription.user_info && subscription.user_info.profile_photo ? subscription.user_info.profile_photo : '/src/assets/img/avatar.svg'"
        alt="Photo" class="avatar">
      <span>{{ subscription.user_info ? subscription.user_info.first_name : 'Пользователь' }}</span>
    </div>
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
      subscriptions: []
    };
  },

  mounted() {
    this.fetchUserSubscriptions()
  },

  methods: {
    fetchUserSubscriptions() {
      const accessToken = localStorage.getItem('accessToken');

      axios.get(`${this.backendURL}/me/subscriptions`, {
        headers: {
          Authorization: `Bearer ${accessToken}`
        }
      })
        .then(response => {
          this.subscriptions = response.data; // Заполнение массива целей из ответа сервера

          // Получаем информацию о каждом пользователе, на которого мы подписаны
          this.subscriptions.forEach(subscription => {
            axios.get(`${this.backendURL}/profile/${subscription.publisher_id}`, {
              headers: {
                Authorization: `Bearer ${accessToken}`
              }
            })
              .then(userResponse => {
                // Добавляем информацию о пользователе к объекту подписки
                subscription.user_info = userResponse.data;
              })
              .catch(userError => {
                console.error('Ошибка при получении информации о подписке:', userError);
              });
          });
        })
        .catch(error => {
          console.error('Ошибка при получении подписок:', error);
        });
    },
    showFriendName(subscription) {
      this.$emit('changePage', 'FriendPage');
      this.$emit('friendSelected', subscription.user_info.id);
    }
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

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 15px;
  line-height: 18px;

  /* Диплом/Текс */
  color: #444444;


}

img {
  width: 36px;
  height: 36px;
}

span {
  margin-left: 24px;
}

.avatar {
  border-radius: 20px;
}
</style>
