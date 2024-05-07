<template>
  <div class="headerPc" @click="hideResultsOnClick">
    <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
    <div class="searchLine">
      <img src="/src/assets/img/search.svg" alt="" class="search_img">
      <input type="text" placeholder="Поиск" v-model="searchQuery" @input="searchUsers">
      <div v-if="searchResults.length" class="searchResults">
        <div v-for="(result, index) in searchResults" :key="index" class="searchResult" @click="selectResult(result)">
          <img :src="result.profilePhoto" alt="Profile Photo" v-if="result.profilePhoto" class="search_avatar">
          <span>{{ result.name }}</span>
        </div>
      </div>
    </div>
    <div class="menu_buttons">
      <p id="p1">Популярное</p>
      <p id="p2">Для тебя</p>
    </div>
    <div class="user">
      <img :src="user.profile_photo" alt="Photo" v-if="user.profile_photo" class="avatar">
      <p>{{ user.first_name }} {{ user.last_name }}</p>
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
      user: {},
      searchQuery: '',
      searchResults: [] // Добавляем массив для хранения результатов поиска
    };
  },
  created() {
    this.fetchUserData();
  },
  mounted() {
    document.addEventListener('click', this.hideResultsOnClickOutside);
  },
  beforeDestroy() {
    document.removeEventListener('click', this.hideResultsOnClickOutside);
  },
  methods: {
    fetchUserData() {
      // Получение токена доступа из локального хранилища
      const accessToken = localStorage.getItem('accessToken');

      // Выполнение запроса к серверу с токеном доступа
      axios.get(`http://158.160.11.10:8000/me`, {
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
    searchUsers() {
      const query = this.searchQuery.trim();
      if (query === '') {
        this.searchResults = []; // Очищаем результаты поиска, если запрос пустой
        return;
      }
      axios.get(`http://158.160.11.10:8000/search/users/${query}`)
        .then(response => {
          console.log('Результаты поиска:', response.data);
          // Маппим массив пользователей и формируем объекты с именем, фамилией и фотографией профиля
          this.searchResults = response.data.map(user => ({
            name: `${user.first_name} ${user.last_name}`,
            profilePhoto: user.profile_photo,
            id: `${user.id}`
          }));
        })
        .catch(error => {
          console.error('Ошибка при поиске:', error);
        });
    },
    hideResultsOnClick(event) {
      // Если клик был вне меню результатов, скрываем меню
      if (!event.target.closest('.searchLine')) {
        this.searchResults = [];
        this.searchQuery = '';
      }
    },
    selectResult(result) {
      // Действие при выборе результата
      console.log('Выбран результат:', result.id);
      this.$emit('changePage', 'FriendPage');
      this.$emit('friendSelected', result.id);
      this.searchResults = [];
      this.searchQuery = '';
    },
    hideResultsOnClickOutside(event) {
      // Если клик был вне компонента поиска, скрываем меню
      if (!this.$el.contains(event.target)) {
        this.searchResults = [];
      }
    }
  }
};
</script>

<style scoped>
.searchResults {
  margin-top: 43px;
  width: 300px;
  position: absolute;

  border: 1px solid #ccc;
  padding-left: 32px;
  padding-bottom: 24px;
  overflow-y: auto;
  /* добавление полосы прокрутки, если результаты слишком длинные */
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  background-color: #fff9f9;

  border-radius: 20px;


}

.searchResult {
  cursor: pointer;
  width: 327px;

  align-items: center;

  margin-top: 24px;

}

.searchResult:hover {
  background-color: #f0f0f0;
}

.search_avatar {
  width: 36px;
  height: 36px;
  border-radius: 20px;

  margin-right: 24px;
}

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
  border-radius: 20px;

}

.user p {
  margin-top: 12px;
  margin-left: 24px;
  height: 19px;
  font-size: 16px;
}

.searchLine input {
  width: 77%;
  background-color: #E0E6EF;
  border: none;
  padding-left: 12px;

  font-size: 14px;
  outline: none;
}

.search_img {
  height: 20px;
  width: 20px;

  margin-top: 9px;
  margin-left: 12px;
}

.searchLine {
  height: 41px;
  display: flex;
  background-color: #E0E6EF;

  border-radius: 20px;
}
</style>