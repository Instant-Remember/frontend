<template>
  <div class="postEditor">
    <input placeholder="Чем вы хотите поделиться?">
    <button class="tbutton" @click="showPopup = true">Добавить пост</button>

    <popup v-if="showPopup" @close="showPopup = false" class="popupPost">
      <button class="chooseGoal">Выберите цель</button>
      <input type="text" placeholder="Чем вы хотите поделиться?" class="postDescription">
      <progressBar></progressBar>
      <button @click="$emit('close')" class="createPost">Создать</button>
    </popup>

  </div>
</template>
  
<script>
import Popup from './Popup.vue'
import progressBar from './progressBar.vue';


export default {
  components: { Popup, progressBar},

  data() {
    return {
      showPopup: false
    };
  }
}
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

.popupPost{
height: 249px;
}

.postDescription {

  width: 609px;
  height: 79px;
  margin-top: 12px;
  margin-left: 40px;
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
  background-color: #6133B4;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;

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
  background-color: #6133B4;

  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 17px;

  cursor: pointer;
}
</style>
  