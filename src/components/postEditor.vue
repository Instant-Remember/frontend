<template>
  <div class="postEditor">
    <input placeholder="Чем вы хотите поделиться?">
    <button class="tbutton" @click="showPopup = true">Добавить пост</button>

    <popup v-if="showPopup" @close="showPopup = false" class="popupPost">
      <div class="dropdown-container">
        <button class="chooseGoal" @click="toggleDropdown">{{ selectedGoal }}</button>
        <ul v-if="isDropdownOpen" class="dropdown-menu" :style="{ width: dropdownWidth + 'px' }" @click="selectGoal">
          <li class="dropdown-item">Кибермонстр</li>
          <li class="dropdown-item">Мой первый стартап</li>
        </ul>
      </div>

      <input type="text" placeholder="Чем вы хотите поделиться?" class="postDescription">
      <progressBar></progressBar>
      <button @click="$emit('close')" class="createPost">Создать</button>
    </popup>
  </div>
</template>

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
  top: auto; /* Сбросим значение top */
  bottom: 0%; /* Расположим меню над кнопкой */
  left: 32px;
  width: inherit;
  box-shadow: 0px 4px 20px rgba(224, 230, 239, 0.8);
  backdrop-filter: blur(8px);
  border-radius: 20px 20px 10px 10px;
  padding: 8px;
  background-color: #FFF9F9;
  margin-top: 0; /* Уберем верхний отступ */
  margin-bottom: 36px; /* Оставим нижний отступ для пункта выбора */
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

<script>
import Popup from './Popup.vue';
import progressBar from './progressBar.vue';

export default {
  components: { Popup, progressBar },

  data() {
    return {
      showPopup: false,
      isDropdownOpen: false,
      selectedGoal: 'Выберите цель',
      dropdownWidth: 218,
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
    },
  },
};
</script>
