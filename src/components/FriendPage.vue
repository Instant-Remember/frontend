<template>
  <div class="profileHeader">

    <photo>
      <img src="/src/assets/img/avatar.svg" alt="">
    </photo>

    <description>
      <user class="userheader">
        <div class="test1">{{ friendInfo.first_name }} {{ friendInfo.last_name }}</div>
        <div class="test2" id="test">{{ friendInfo.about }}</div>
      </user>
      <target>
        <div>{{ countGoals() }}</div>
        <img src="/src/assets/img/navMenu/targetV.svg" alt="">
      </target>

      <div class="nametag">@{{ friendInfo.username }}</div>

      <subs>
        <div class="subscribers">{{ countSubscribers() }} подписчиков</div>
        <div class="subscribe">{{ countSubscribtions() }} Подписок</div>
      </subs>

      <button @click="followUser">{{ isSubscribed ? 'Вы подписаны' : 'Подписаться' }}</button>

    </description>
  </div>

  <div class="posts">
    <div class="post" v-for="(post, index) in posts" :key="index">

      <user class="userpost">
        <img src="/src/assets/img/avatar.svg" alt="">
        <div class="username">{{ friendInfo.first_name }} {{ friendInfo.last_name }}</div>
      </user>

      <div class="mainpost">
        <div class="targetname">{{ getGoalName(post.goal_id) }}</div>
        <div class="description">{{ post.text }}</div>
      </div>

      <progressBar class="progres"></progressBar>

      <date>Неделю назад</date>
      <society>
        <button class="likes" @click="likePost(post.id)">
          <img src="/src/assets/img/likes.svg" alt="">
          <div class="like">{{ post.id }}</div>
        </button>
        <button class="comments"><img src="/src/assets/img/comments.svg" alt=""></button>
      </society>
    </div>
  </div>

</template>

<script>
import axios from 'axios';
import progressBar from './progressBar.vue'

export default {
  components: { progressBar },

  props: {
    friendName: String,
    backendURL: String
  },
  data() {
    return {
      friendInfo: {},
      friendGoals: [],
      friendSubscribers: [],
      friendSubscriptions: [],
      posts: [], // Массив для хранения постов пользователя
      isSubscribed: false, // Начальное состояние: пользователь не подписан
    };
  },
  created() {
    this.fetchFriendInfo();
    this.fetchFriendGoals();
    this.fetchFriendSubscribers();
    this.fetchFriendSubscriptions();
    this.fetchUserPosts(); // Выполнение запроса на получение постов пользователя при загрузке компонента
    this.isSubscription();// Проверка подписки на друга при загрузке компонента
  },
  methods: {
    fetchFriendInfo() {
      // Отправляем запрос на бэкенд для получения данных о друге по его ID
      axios.get(`http://130.193.34.79:8000/profile/${this.friendName}`)
        .then(response => {
          this.friendInfo = response.data;
        })
        .catch(error => {
          console.error('Ошибка при получении данных о друге:', error);
        });
    },
    fetchFriendGoals() {
      // Отправляем запрос на бэкенд для получения целей друга
      axios.get(`http://130.193.34.79:8000/profile/${this.friendName}/goals?user_id=${this.friendName}`)
        .then(response => {
          console.log('Цели друга:', response.data);
          this.friendGoals = response.data;
        })
        .catch(error => {
          console.error('Ошибка при получении целей друга:', error);
        });
    },
    countGoals() { // Метод для подсчета количества целей пользователя
      return this.friendGoals.length;
    },
    getGoalName(goalId) { // Новый метод для получения имени цели по её id
      const goal = this.friendGoals.find(goal => goal.id === goalId);
      return goal ? goal.name : 'Цель не найдена';
    },
    fetchFriendSubscribers() {
      axios.get(`http://130.193.34.79:8000/profile/${this.friendName}/subscribers`)
        .then(response => {
          console.log('Подписчики:', response.data);
          this.friendSubscribers = response.data; // Заполнение массива целей из ответа сервера
        })
        .catch(error => {
          console.error('Ошибка при получении подписчиков:', error);
        });
    },
    countSubscribers() {
      return this.friendSubscribers.length;
    },
    fetchFriendSubscriptions() {
      axios.get(`http://130.193.34.79:8000/profile/${this.friendName}/subscriptions`)
        .then(response => {
          console.log('Подписки:', response.data);
          this.friendSubscriptions = response.data; // Заполнение массива целей из ответа сервера
        })
        .catch(error => {
          console.error('Ошибка при получении подписок:', error);
        });
    },
    countSubscribtions() {
      return this.friendSubscriptions.length;
    },
    fetchUserPosts() {
      axios.get(`http://130.193.34.79:8000/profile/${this.friendName}/posts`)
        .then(response => {
          console.log('Посты друга:', response.data);
          this.posts = response.data; // Заполнение массива постами из ответа сервера
        })
        .catch(error => {
          console.error('Ошибка при получении постов:', error);
        });
    },
    followUser() {
      // Получаем access token из localStorage
      const accessToken = localStorage.getItem('accessToken');
      // Формируем заголовок авторизации
      const config = {
        headers: {
          'Authorization': `Bearer ${accessToken}`
        }
      };

      // Выполняем GET запрос к эндпоинту подписки на пользователя
      axios.get(`http://130.193.34.79:8000/profile/${this.friendName}/follow`, config)
        .then(response => {
          console.log('Пользователь успешно подписан:', response.data);
          this.isSubscribed = true; // Устанавливаем состояние подписки в true после успешной подписки
          // Дополнительные действия при успешной подписке, если необходимо
        })
        .catch(error => {
          console.error('Ошибка при подписке на пользователя:', error);
        });
    },
    isSubscription() {
      // Получаем access token из localStorage
      const accessToken = localStorage.getItem('accessToken');
      // Формируем заголовок авторизации
      const config = {
        headers: {
          'Authorization': `Bearer ${accessToken}`
        }
      };

      // Выполняем GET запрос к эндпоинту проверки подписки на пользователя
      axios.get(`http://130.193.34.79:8000/profile/${this.friendName}/is_subscription`, config)
        .then(response => {
          if (response.data.status === 'ok') {
            this.isSubscribed = response.data.result; // Обновляем состояние подписки в соответствии с результатом
          } else {
            console.error('Ошибка при проверке подписки:', response.data);
          }
        })
        .catch(error => {
          console.error('Ошибка при проверке подписки:', error);
        });
    },
    likePost(postId) {
      // Получаем access token из localStorage
      const accessToken = localStorage.getItem('accessToken');
      // Формируем заголовок авторизации
      const config = {
        headers: {
          'Authorization': `Bearer ${accessToken}`
        }
      };

      // Выполняем GET запрос к эндпоинту проверки подписки на пользователя
      axios.get(`http://130.193.34.79:8000/post/${postId}/like`)
        .then(response => {
          console.log('Поставлен лайк', response.data);
        })
        .catch(error => {
          console.error('Ошибка при лайке', error);
        });
    },
    getLikes(postID) {
      axios.get(`http://130.193.34.79:8000/post/${postId}/likes`)
        .then(response => {
          likes = response.data
          console.log('Kfqrb:', response.data);
        })
        .catch(error => {
          console.error('Ошибка при проверке лайков', error);
        });
    }
  },
}
</script>

<style scoped>
.profileHeader {
  width: 757px;
  height: 193px;
  margin-left: 397px;

  background-image: url("/src/assets/img/profileHeader.svg");
  border-radius: 20px;

  display: flex;
}

description {

  display: grid;
  grid-template-columns: 157px 133px 123px 159px;
  grid-template-rows: 45px 18px;

  width: 548px;
  height: 63px;

  margin-left: 24px;
  margin-top: 115px;

}

photo {
  width: 130px;
  height: 130px;
  border-radius: 60px;

  background-color: #F6F1F1;

  margin-top: 46px;
  margin-left: 31px;
}

.userheader {
  width: 119px;
  height: 39px;

  grid-row: 1;
  grid-column: 1;
}

.test1 {
  font-family: 'Inter';
  font-style: medium;
  font-weight: 500;
  font-size: 16px;
  line-height: 19px;

  color: #444444;

  height: 19px;
  width: 550px;

  margin-top: 10px;
}

.test2 {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 300;
  font-size: 14px;
  line-height: 17px;

  color: #444444;
  margin-top: 8px;
}

target {

  width: 54px;
  height: 36px;

  display: flex;

  grid-row: 1;
  grid-column: 2;
}

target div {

  font-family: 'Jost';
  font-style: normal;
  font-weight: 200;
  font-size: 24px;
  line-height: 35px;
  display: flex;
  align-items: center;

  color: #444444;

}

target img {
  margin-left: 4px;

  width: 36px;
  height: 36px;
}

.nametag {
  font-family: 'Comfortaa';
  font-style: normal;
  font-weight: 300;
  font-size: 16px;
  line-height: 18px;

  display: flex;
  align-items: center;

  color: #004ABA;

  grid-row: 1;
  grid-column: 3;

  height: 18px;
  margin-top: 16px;
  margin-left: -18px;
}

description button {
  grid-row: 1 / 2;
  grid-column: 4;
}

subs {
  grid-row: 2;
  grid-column: 2 / 3;

  display: flex;

  width: 219px;
  height: 16px;

  /* Group 336494 */

  font-family: 'Inter';
  font-style: normal;
  font-weight: 300;
  font-size: 13px;
  line-height: 16px;

  color: #8C8C8C;
}

.subscribe {
  margin-left: 24px;
}

button {
  width: 135px;
  height: 41px;

  border-radius: 20px;

  border-color: #6133B4;
  color: #6133B4;

  outline: none;

  margin-top: 14px;
  cursor: pointer;
}


.post {
  width: 757px;
  height: auto;
  margin-left: 397px;
  margin-top: 16px;

  border-radius: 20px;
  background-color: #fff;

  display: grid;
  grid-template-columns: 69px 555px 133px;
  grid-template-rows: 62px 53px 40px;
}

.userpost {
  grid-row: 1;
  grid-column: 1 / 2;

  display: flex;

  width: 624px;

}

user img {
  width: 42px;
  height: 42px;

  margin: 16px 16px;
}

.username {
  margin-top: 27px;
  width: 118px;
  height: 19px;


  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
  line-height: 19px;
  display: flex;
  align-items: center;

  color: #444444;
  width: 550px;

}

.mainpost {
  grid-row: 2;
  grid-column: 2 / 3;

  margin-left: 5px;
}

.targetname {

  width: auto;
  height: 18px;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 600;
  font-size: 15px;
  line-height: 18px;
  display: flex;
  align-items: center;
  text-align: center;

  /* Диплом/Акцентный */
  color: #6133B4;


  /* Inside auto layout */
  flex: none;
  order: 0;
  flex-grow: 0;

}

.description {

  width: 608px;
  height: auto;

  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
  line-height: 22px;

  color: #444444;

  margin-top: 4px;
}

.progres {
  grid-row: 3;
  grid-column: 1 / 2;

  display: flex;

  width: 533px;
  margin-top: 15px;
}


date {
  grid-row: 1;
  grid-column: 3;

  margin-top: 27px;
  margin-left: 7px;

  width: 109px;
  height: 19px;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 200;
  font-size: 16px;
  line-height: 19px;
  display: flex;
  align-items: center;

  color: #444444;
}

society {
  grid-row: 3;
  grid-column: 3;

  display: flex;

  margin-left: 34px;
  margin-top: 3px;
}

.likes {
  display: flex;
  width: 40px;
  height: 19px;

  padding: 0;
  background-color: #fff;
  border: none;
  cursor: pointer;
}

.like {
  margin-left: 4px;

  width: 18px;
  height: 18px;

  font-family: 'Roboto';
  font-style: normal;
  font-weight: 200;
  font-size: 15px;
  line-height: 18px;

  display: flex;
  align-items: center;

  color: #444444;


}

.comments {
  width: 18px;
  height: 18px;
  margin-left: 24px;
  padding: 0;
  background-color: #fff;
  border: none;
  cursor: pointer;
}
</style>
