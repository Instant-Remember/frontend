<template>
    <div class="navMenuOptions">
      <container>
        <div class="cover" :class="{ active: activeMenuItem === 'cover' }" @click="changeToCover">
          <text>Обложка</text>
        </div>
        <div class="profile" :class="{ active: activeMenuItem === 'profile' }" @click="changeToProfile">
          <text>Профиль</text>
        </div>
        <div class="info" :class="{ active: activeMenuItem === 'info' }" @click="changeToInfo">
          <text>Информация</text>
        </div>
        <div class="privacy" :class="{ active: activeMenuItem === 'privacy' }" @click="changeToPrivacy">
          <text>Конфиденциальность</text>
        </div>
        <div class="language" :class="{ active: activeMenuItem === 'language' }" @click="changeToLanguage">
          <text>Язык</text>
        </div>
      </container>
    </div>
  </template>
  
  <style scoped>
  
  .navMenuOptions {
    background-color: #fff9f9;
    height: 193px;
    width: 313px;
    border-radius: 20px;
  }
  
  .cover,
  .profile,
  .privacy,
  .language,
  .info {
    display: flex;
    height: 19px;
    cursor: pointer;
    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;
    color: #444444;
  }
  
  .cover.active,
  .profile.active,
  .info.active,
  .privacy.active,
  .language.active {
    color: #6133B4;
  }
  
  .cover {
    margin-top: 17px;
  }
  
  .profile {
    margin-top: 17px;
  }

  .info {
    margin-top: 17px;
  }
  
  .privacy {
    margin-top: 17px;
  }
  
  .language {
    margin-top: 17px;
  }
  
  text {
    margin-left: 16px;
  }
  </style>
  
  <script>
  export default {
    methods: {
      changeToCover() {
        this.activeMenuItem = 'cover';
      },
      changeToProfile() {
        this.activeMenuItem = 'profile';
      },
      changeToInfo() {
        this.activeMenuItem = 'info';
      },
      changeToPrivacy() {
        this.activeMenuItem = 'privacy';
      },
      changeToLanguage() {
        this.activeMenuItem = 'language';
      },
    },
    data() {
      return {
        activeMenuItem: 'cover',
      };
    },
  };
  </script>
  