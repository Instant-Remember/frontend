<template>
  <div class="postEditor">
    <input type="text" v-model="postText" placeholder="Чем вы хотите поделиться?">
    <button class="tbutton" @click="showPopup = true">Добавить пост</button>

    <popup v-if="showPopup" @close="showPopup = false" class="popupPost">
      <div class="dropdown-container">
        <button class="chooseGoal" @click="toggleDropdown">{{ selectedGoal }}</button>
        <ul v-if="isDropdownOpen" class="dropdown-menu" :style="{ width: dropdownWidth + 'px' }" @click="selectGoal">
          <li class="dropdown-item" v-for="(goal, index) in goals" :key="index">{{ goal.name }}</li>
        </ul>
      </div>

      <input type="text" v-model="postText" placeholder="Чем вы хотите поделиться?" class="postDescription">
      <progressBar></progressBar>
      <button @click="createPost" class="createPost">Создать</button>
    </popup>
  </div>
</template>

<script>
import Popup from './Popup.vue';
import progressBar from './progressBar.vue';
import axios from 'axios';

export default {
  components: { Popup, progressBar },

  data() {
    return {
      showPopup: false,
      isDropdownOpen: false,
      selectedGoal: 'Выберите цель',
      dropdownWidth: 218,
      goals: [],
      selectedGoalId: null,
      postText: ''
    };
  },

  methods: {
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
    selectGoal(event) {
      const selectedText = event.target.innerText;
      this.selectedGoal = selectedText;
      this.isDropdownOpen = false;

      // Получаем выбранный goal_id
      const selectedGoalIndex = this.goals.findIndex(goal => goal.name === selectedText);
      this.selectedGoalId = this.goals[selectedGoalIndex].id;
    },
    fetchGoals() {
      // Получение токена доступа из локального хранилища
      const accessToken = localStorage.getItem('accessToken');

      // Выполнение запроса к серверу для получения списка целей пользователя
      axios.get('http://51.250.111.113:8000/me/goals', {
          headers: {
            Authorization: `Bearer ${accessToken}`
          }
        })
        .then(response => {
          console.log('Цели пользователя:', response.data);
          this.goals = response.data;
        })
        .catch(error => {
          console.error('Ошибка при получении целей:', error);
        });
    },
    createPost() {
      // Получаем выбранный goal_id
      const selectedGoalId = this.selectedGoalId;

      // Получаем текст поста из поля ввода
      const postText = this.postText;

      // Проверяем, что goal_id и текст поста не пустые
      if (!selectedGoalId || !postText) {
        console.error('Goal ID or post text is empty');
        return;
      }

      // Формируем объект данных для отправки на сервер
      const postData = {
        text: postText,
        progress: 15,
        goal_id: selectedGoalId,
      };

      // Получаем токен доступа из локального хранилища
      const accessToken = localStorage.getItem('accessToken');

      // Отправляем запрос на сервер для создания поста
      axios.post('http://51.250.111.113:8000/post/', postData, {
          headers: {
            Authorization: `Bearer ${accessToken}`,
            'Content-Type': 'application/json'
          }
        })
        .then(response => {
          console.log('Пост успешно создан:', response.data);
          // Дополнительные действия при необходимости, например, обновление списка постов
          // или закрытие всплывающего окна
        })
        .catch(error => {
          console.error('Ошибка при создании поста:', error);
        });
    }
  },
  mounted() {
    // Выполнение запроса на получение списка целей при загрузке компонента
    this.fetchGoals();
  }
};
</script>


<style>
.postEditor {
  width: 757px;
  height: 52px;
  margin-left: 397px;
  margin-top: 16px;
  border-radius: 20px;
  background-color: #fff;
  display: flex;
}

input {
  margin-left: 15px;
  border: none;
  outline: none;
  width: 600px;
  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;
  color: #8C8C8C;
  transition: border-bottom 0.3s;
  cursor: text;
}

.tbutton {
  width: 135px;
  height: 41px;
  margin-top: 6px;
  margin-right: 7px;
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

button:active {
  background-color: #0056b3;
}

.popupPost {
  height: 249px;
}

.postDescription {
  width: 609px;
  height: 79px;
  margin-top: 12px;
  margin-left: 40px;
}

.dropdown-container {
  position: relative;
}

.chooseGoal {
  width: 218px;
  height: 32px;
  margin-top: 16px;
  margin-left: 32px;
  background: #6133B4;
  border-radius: 10px;
  border: none;
  outline: none;
  color: #fff;
  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;
  cursor: pointer;
}

.dropdown-menu {
  list-style: none;
  position: absolute;
  top: auto;
  /* Сбросим значение top */
  bottom: 0%;
  /* Расположим меню над кнопкой */
  left: 32px;
  width: inherit;
  box-shadow: 0px 4px 20px rgba(224, 230, 239, 0.8);
  backdrop-filter: blur(8px);
  border-radius: 20px 20px 10px 10px;
  padding: 8px;
  background-color: #FFF9F9;
  margin-top: 0;
  /* Уберем верхний отступ */
  margin-bottom: 36px;
  /* Оставим нижний отступ для пункта выбора */
}

.dropdown-item {
  margin-top: 36px;
  margin-bottom: 36px;
  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 15px;
  line-height: 18px;
  display: flex;
  align-items: center;
  color: #444444;
  cursor: pointer;
}

.createPost {
  margin-top: 12px;
  margin-left: 220px;
  width: 246px;
  height: 49px;
  background: #6133B4;
  border-radius: 20px;
  border: none;
  outline: none;
  color: #fff;
  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;
  cursor: pointer;
}
</style>
