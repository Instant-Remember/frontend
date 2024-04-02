<template>
  <body>
    <header>
      <component :is="headerComponent"></component>
    </header>
    <aside v-if="!isAuthWindowOpen" :class="{ 'isOptionsPage': isOptionsPage }">
      <navMenu @changePage="changePage"></navMenu>
    </aside>
    <friendsMenu v-if="!isAuthWindowOpen && !isOptionsPage" @changePage="changePage" class="friendsMenu"></friendsMenu>
    <navMenuOptions v-else-if="isOptionsPage" :is="navMenuOptions"></navMenuOptions>
    <section class="main">
      <component :is="currentPage" v-if="currentPage !== 'FriendPage'" @loginSuccess="loginSuccess"
        @registerSuccess="registerSuccess" @goToRegisterPage="goToRegisterPage" @goToAuthPage="goToAuthPage"
        @openPasswordRecovery="openPasswordRecovery" />
      <friend-page v-else :friendName="friendName"></friend-page>
    </section>
  </body>
</template>

<script>
import headerPc from './components/headerPc.vue'
import headerPcAuth from './components/headerPcAuth.vue'
import navMenu from './components/navMenu.vue'
import friendsMenu from './components/friendsMenu.vue'
import userPage from './components/userPage.vue'
import feedPage from './components/feedPage.vue'
import goalsPage from './components/goalsPage.vue'
import optionsPage from './components/optionsPage.vue'
import navMenuOptions from './components/navMenuOptions.vue'
import FriendPage from './components/FriendPage.vue'
import HeaderPcAuth from './components/headerPcAuth.vue'
import AuthPage from './components/AuthPage.vue'
import RegisterPage from './components/RegisterPage.vue'
import PasswordRecoveryPage from './components/PasswordRecoveryPage.vue'
import NavMenuOptions from './components/navMenuOptions.vue'

export default {
  components: {
    headerPc,
    navMenu,
    friendsMenu,
    navMenuOptions,
    headerPcAuth,
    userPage,
    goalsPage,
    feedPage,
    optionsPage,
    FriendPage,
    HeaderPcAuth,
    AuthPage,
    RegisterPage,
    PasswordRecoveryPage,
    NavMenuOptions
  },

  data() {
    return {
      currentPage: 'AuthPage', //поменять на AuthPage
      isAuthenticated: false, //поменять на false
      isRegisterMode: false,
      isAuthWindowOpen: true, //поменять на true 
      isOptionsPage: false,
      friendName: '',
    };
  },
  computed: {
    headerComponent() {
      return this.isAuthenticated ? 'headerPc' : 'headerPcAuth';
    },
  },
  methods: {
    changePage(page, data) {
      if (this.isAuthWindowOpen) return;
      if (!this.isAuthenticated && !this.isRegisterMode) {
        this.currentPage = 'AuthPage';
      } else {
        if (page === 'FriendPage') {
          this.friendName = data.friendName;
        }
        this.currentPage = page;
      }
      this.isOptionsPage = page === 'optionsPage';
    },
    loginSuccess() {
      this.isAuthenticated = true;
      this.isAuthWindowOpen = false;
      this.currentPage = 'userPage';
    },
    registerSuccess() {
      this.isRegisterMode = false;
      this.loginSuccess();
    },
    toggleLoginMode() {
      this.isRegisterMode = false;
      this.currentPage = 'AuthPage';
    },
    toggleRegisterMode() {
      this.isRegisterMode = true;
      this.currentPage = 'RegisterPage';
    },
    goToAuthPage() {
      this.isRegisterMode = false;
      this.currentPage = 'AuthPage';
      this.isOptionsPage = false;
    },
    goToRegisterPage() {
      this.isRegisterMode = true;
      this.currentPage = 'RegisterPage';
      this.isOptionsPage = false;
    },
    openPasswordRecovery() {
      this.currentPage = 'PasswordRecoveryPage';
    },
    recoverPasswordSuccess() {
      this.goToAuthPage();
    },
  },
};
</script>

<style scoped>
body {
  margin: auto;
  width: 1440px;
}

header {
  margin-left: auto;
  margin-right: auto;
  position: fixed;
  opacity: 0.8;
  top: 0;  
}

.main {
  padding-top: 94px;
}

aside {
  position: fixed;
  display: flex;
  margin: 94px 64px;
}

.friendsMenu {
  position: fixed;
  margin-top: 94px;
  margin-left: 1174px;
}

.navMenuOptions {
  position: fixed;
  margin-top: 94px;
  margin-left: 1063px;
}

/* Скрыть friendsMenu на странице настроек */
.isOptionsPage .friendsMenu {
  display: none;
}
</style>
