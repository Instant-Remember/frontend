<template>
  <div class="friendPage">
    <div class="profileHeader">

      <photo>
        <img :src="friendInfo.profile_photo" alt="Photo" v-if="friendInfo.profile_photo" class="avatar">
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
          <img :src="friendInfo.profile_photo" alt="Photo" v-if="friendInfo.profile_photo" class="avatar_post">
          <div class="username">{{ friendInfo.first_name }} {{ friendInfo.last_name }}</div>
        </user>

        <div class="mainpost">
          <div class="targetname">{{ getGoalName(post.goal_id) }}</div>
          <div class="description">{{ post.text }}</div>
        </div>

        <progressBar class="progres"></progressBar>

        <date>{{ formatDate(post.date_create) }}</date>
        <society>
          <button class="likes" @click="likePost(post.id)">
            <img src="/src/assets/img/likes.svg" alt="">
            <div class="like">{{ getLikesCount(post.id) }}</div>
          </button>
          <button class="comments_img"><img src="/src/assets/img/comments.svg" alt=""></button>
        </society>

        <div class="comments">
          <div class="line"></div>
          <div class="container">
            <div class="comment" v-for="comment in post.comments" :key="comment.id">
              <img :src="getUserAvatar(comment.user_id)" alt="" class="avatar_comment">
              <div class="comment_username">{{ getCommentUserName(comment.user_id) }}</div>
              <div class="main_comment">{{ comment.text }}</div>
            </div>
          </div>

          <div class="my_comment">
            <input type="text" class="write_comment" placeholder="Напишите свой комментарий" v-model="newCommentText">
            <button type="button" @click="addComment(post.id)" class="send"><img src="/src/assets/img/send.svg"
                alt=""></button>
          </div>
        </div>

      </div>
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
      users: {},
      likes: {},
      newCommentText: ''
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
    formatDate(dateString) {
      const months = [
        "января", "февраля", "марта", "апреля", "мая", "июня",
        "июля", "августа", "сентября", "октября", "ноября", "декабря"
      ];

      const dateObj = new Date(dateString);
      const day = dateObj.getDate();
      const monthIndex = dateObj.getMonth();
      const year = dateObj.getFullYear();

      return `${day} ${months[monthIndex]} ${year}`;
    },
    fetchFriendInfo() {
      // Отправляем запрос на бэкенд для получения данных о друге по его ID
      axios.get(`${this.backendURL}/profile/${this.friendName}`)
        .then(response => {
          this.friendInfo = response.data;
        })
        .catch(error => {
          console.error('Ошибка при получении данных о друге:', error);
        });
    },
    fetchFriendGoals() {
      // Отправляем запрос на бэкенд для получения целей друга
      axios.get(`${this.backendURL}/profile/${this.friendName}/goals?user_id=${this.friendName}`)
        .then(response => {
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
      axios.get(`${this.backendURL}/profile/${this.friendName}/subscribers`)
        .then(response => {
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
      axios.get(`${this.backendURL}/profile/${this.friendName}/subscriptions`)
        .then(response => {
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
      axios.get(`${this.backendURL}/profile/${this.friendName}/posts`)
        .then(response => {
          this.posts = response.data; // Заполнение массива постами из ответа сервера

          this.posts.forEach(post => {
            this.getComments(post.id);
            this.getLikes(post.id);
          });
        })
        .catch(error => {
          console.error('Ошибка при получении постов:', error);
        });
    },
    followUser() {
      const accessToken = localStorage.getItem('accessToken');
      const config = {
        headers: {
          'Authorization': `Bearer ${accessToken}`
        }
      };

      // Выполняем GET запрос к эндпоинту подписки на пользователя
      axios.get(`${this.backendURL}/profile/${this.friendName}/follow`, config)
        .then(response => {
          this.isSubscribed = true; // Устанавливаем состояние подписки в true после успешной подписки
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
      axios.get(`${this.backendURL}/profile/${this.friendName}/is_subscription`, config)
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
      const accessToken = localStorage.getItem('accessToken');

      axios.get(`${this.backendURL}/post/${postId}/like`, {
        headers: {
          'accept': 'application/json',
          'Authorization': `Bearer ${accessToken}`
        }
      })
        .then(response => {
          this.getLikes(postId);
        })
        .catch(error => {
          console.error('Ошибка при отправке лайка:', error);
        });
    },
    getLikes(postId) {
      axios.get(`${this.backendURL}/post/${postId}/likes`, {
        headers: {
          'accept': 'application/json'
        }
      })
        .then(response => {
          this.likes[postId] = response.data;
        })
        .catch(error => {
          console.error('Ошибка при получении данных о лайках поста:', error);
        });
    },
    getLikesCount(postId) {
      const likes = this.likes[postId];
      return likes ? likes.length : 0;
    },
    getComments(postId) {
      axios.get(`${this.backendURL}/post/${postId}/comments`, {
        headers: {
          'accept': 'application/json'
        }
      })
        .then(response => {
          this.posts.find(post => post.id === postId).comments = response.data;

          // Для каждого комментария получаем данные о пользователе
          response.data.forEach(comment => {
            this.getUser(comment.user_id);
          });
        })
        .catch(error => {
          console.error('Ошибка при получении данных о комментариях поста:', error);
        });
    },
    getUser(userId) {
      axios.get(`${this.backendURL}/profile/${userId}`, {
        headers: {
          'accept': 'application/json'
        }
      })
        .then(response => {
          this.users[userId] = response.data;
        })
        .catch(error => {
          console.error('Ошибка при получении данных о пользователе:', error);
        });
    },
    getCommentUserName(userId) {
      const user = this.users[userId];
      return user ? `${user.first_name} ${user.last_name}` : '';
    },
    getUserAvatar(userId) {
      const user = this.users[userId];
      return user ? user.profile_photo : '/src/assets/img/avatar.svg'; // Если ссылка на аватар отсутствует, используем заглушку
    },
    addComment(postId) {
      const accessToken = localStorage.getItem('accessToken');
      const newCommentText = this.newCommentText.trim();

      if (newCommentText) {
        axios.post(`${this.backendURL}/comment/create`, {
          text: newCommentText,
          post_id: postId
        }, {
          headers: {
            'accept': 'application/json',
            'Authorization': `Bearer ${accessToken}`,
            'Content-Type': 'application/json'
          }
        })
          .then(response => {
            // Опционально: обновление списка комментариев
            this.getComments(postId);
            // Очистка поля ввода после успешной отправки
            this.newCommentText = '';
          })
          .catch(error => {
            console.error('Ошибка при добавлении комментария:', error);
          });
      }
    },
  },
}
</script>

<style scoped>
@media (max-width: 425px) {
  .profileHeader {
    width: 375px !important;
    height: 319px !important;

    margin-left: 0px !important;
    margin-top: 135px !important;

    background-image: url("/src/assets/img/profileHeaderMob.svg") !important;


  }


  photo {
    margin-left: 123px !important;
    margin-top: 50px !important;

  }

  description {
    width: 375px !important;
    height: 207px;

    margin-left: -253px !important;
    margin-top: 111px !important;

    display: grid !important;
    grid-template-columns: 122px 131px 122px !important;
    grid-template-rows: 71px 47px 35px 54px !important;
  }

  .userheader {
    grid-column: 2 !important;
    grid-row: 2 !important;

    width: 253px !important;
    height: 47px !important;
    display: flex !important;
    flex-direction: column !important;

    margin-top: 5px !important;
  }

  .test1 {
    margin-top: 0 !important;
    margin-left: 0 !important;
    width: fit-content !important;
  }

  .test2 {
    margin-top: 2px !important;
    margin-left: -120px !important;

    text-align: center !important;
    width: 370px !important;

  }

  target {
    grid-column: 1 !important;
    grid-row: 1 !important;

    width: 122px !important;
    height: 49px !important;
    margin-top: 9px !important;

    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
  }

  target div {
    margin-top: 0 !important;
  }

  target img {
    margin-top: 0 !important;
  }

  .nametag {
    grid-column: 3 !important;
    grid-row: 1 !important;

    margin-top: 22px !important;
    margin-left: 22px !important;
    height: auto !important;
    width: 80px;
    word-wrap: break-word !important;
  }

  description button {
    grid-column: 1 2 3 !important;
    grid-row: 4 !important;

    margin-left: -340px !important;
    width: 310px !important;
    margin-top: 0px !important;
  }

  subs {
    grid-column: 1 2 3 !important;
    grid-row: 3 !important;

    margin-top: 4px !important;

    width: 375px !important;
    margin-left: -120px !important;
  }

  .subscribers {
    margin-left: 16px !important;
  }

  .subscribe {
    margin-left: 190px !important;
  }

  .posts {
    width: 375px !important;

    margin-left: 0px !important;
    height: calc(100vh + 550px) !important;
  }

  .post {
    width: 343px !important;
    margin-left: 16px !important;

    display: grid !important;
    grid-template-columns: 223px 120px !important;
    grid-template-rows: 62px auto 33px auto !important;
  }

  .userpost {
    grid-row: 1 !important;
    grid-column: 1  !important;

    display: inline;
    justify-content: center;

    
  }


  .username { 
   display: flex !important;

   margin-left: -400px;
 
    width: fit-content !important;
  }

  .avatar_post{
    margin-left: -400px !important;
  }

  .mainpost {
    grid-row: 2 !important;
    grid-column: 1 / 2 !important;

    margin-left: 16px !important;
  }


  .description {
    width: 311px !important;
    height: auto !important;
    margin-top: 4px !important;
    word-wrap: break-word !important;
  }

  .progres {
    grid-row: 3 !important;
    grid-column: 1 !important;

    width: 250px !important;
    margin-top: 0px !important;
    margin-left: -15px !important;
  }


  date {
    grid-row: 1 !important;
    grid-column: 2 !important;

    margin-top: 27px !important;
  }

  society {
    grid-row: 3 !important;
    grid-column: 2 !important;

  }

  .container {
    max-height: 120px !important;
    width: 343px !important;

    scrollbar-width: none !important;


  }

  .comments {
    grid-row: 4 !important;
  }

  .comments_img {
    width: 18px !important;
    height: 18px !important;
    margin-left: 24px !important;
    padding: 0 !important;
    background-color: #fff !important;
    border: none !important;
    cursor: pointer !important;
  }

  .line {
    width: 343px !important;

    grid-row: 1 !important;
    grid-column: 1 / 2 !important;
  }



  

  .my_comment {
    grid-row: 2 !important;
    grid-column: 1 !important;

    width: 343px !important;
    display: flex !important;
    margin-left: 19px !important;

    margin-top: 5px !important;

    margin-left: 0px !important;
  }

  .write_comment {
    display: flex !important;

    width: 250px !important;

    margin-left: 0 !important;
  }

  .send {
    border: none !important;
    background-color: #fff !important;
  }


  .comment_username {
    width: fit-content !important;
  }

  .main_comment {
    width: 280px !important;
  }
}

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

.avatar_post {
  width: 42px;
  height: 42px;
  border-radius: 65px;

  margin: 16px 16px;
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


  align-items: center;

  color: #004ABA;

  grid-row: 1;
  grid-column: 3;


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


.container {
  max-height: 170px;
  width: 757px;
  overflow-y: auto;
  /* Добавляем вертикальную прокрутку */
  scrollbar-width: none;
}

.posts {
  margin-top: -15px;
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
  grid-template-rows: 62px auto 40px auto;
}

.comments {
  grid-row: 4;
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

  word-wrap: break-word;
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

.comments_img {
  width: 18px;
  height: 18px;
  margin-left: 24px;
  padding: 0;
  background-color: #fff;
  border: none;
  cursor: pointer;
}

.line {
  height: 5px;
  width: 757px;

  grid-row: 1;
  grid-column: 1 / 2;
  background-color: #E8ECF2;
}

.comment {
  grid-row: 1;
  grid-column: 1;

  grid-row: 1;
  display: grid;
  grid-template-columns: 41px 645px;
  grid-template-rows: 40px auto;

}

.avatar {
  width: 130px;
  height: 130px;
  border-radius: 65px;
}

.avatar_comment {
  width: 28px;
  height: 28px;
  grid-row: 1;
  grid-column: 1;
  margin-top: 12px;
  margin-left: 11px;
}

.comment_username {
  grid-row: 1;
  grid-column: 2;

  margin-top: 12px;
  margin-left: 5px;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 15px;
  line-height: 18px;
  display: flex;
  align-items: center;

  color: #444444;
}

.main_comment {
  grid-row: 2;
  grid-column: 2;
  display: flex;
  margin-left: 4px;


  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
  line-height: 22px;

  color: #444444;
}

.my_comment {
  grid-row: 2;
  grid-column: 1;

  width: 687px;
  display: flex;
  margin-left: 19px;

  margin-top: 5px;
}

.write_comment {


  display: flex;


  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 15px;
  line-height: 18px;
  display: flex;
  align-items: center;

  color: #8C8C8C;
}

.send {
  border: none;
  background-color: #fff;
}
</style>
