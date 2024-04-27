<template>

  <body>
    <header>
      <component :is="headerComponent" :backendURL="backendURL"></component>
    </header>
    <aside v-if="!isAuthWindowOpen" :class="{ 'isOptionsPage': isOptionsPage }" :backendURL="backendURL">
      <navMenu :backendURL="backendURL" @changePage="changePage"></navMenu>
    </aside>
    <friendsMenu :backendURL="backendURL" @friendSelected="friendSelected" v-if="!isAuthWindowOpen && !isOptionsPage"
      @changePage="changePage" class="friendsMenu"></friendsMenu>
    <navMenuOptions v-else-if="isOptionsPage" :is="navMenuOptions"></navMenuOptions>
    <section class="main">
      <component :is="currentPage" :friendName="friendName" :backendURL="backendURL" @loginSuccess="loginSuccess"
        @registerSuccess="registerSuccess" @goToRegisterPage="goToRegisterPage" @goToAuthPage="goToAuthPage"
        @openPasswordRecovery="openPasswordRecovery" />
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
import HeaderPcAuth from './components/headerPcAuth.vue'
import AuthPage from './components/AuthPage.vue'
import RegisterPage from './components/RegisterPage.vue'
import PasswordRecoveryPage from './components/PasswordRecoveryPage.vue'
import NavMenuOptions from './components/navMenuOptions.vue'
import FriendPage from './components/FriendPage.vue'

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
    HeaderPcAuth,
    AuthPage,
    RegisterPage,
    PasswordRecoveryPage,
    NavMenuOptions,
    FriendPage,
  },

  data() {
    return {
      currentPage: 'AuthPage',
      isAuthenticated: false,
      isRegisterMode: false,
      isAuthWindowOpen: true,
      isOptionsPage: false,
      friendName: '',
      backendURL: 'http://158.160.18.182:8000'
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
    friendSelected(name) {
      this.friendName = name;
    },
    testFriendName() {
      console.log(this.friendName);
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

.isOptionsPage .friendsMenu {
  display: none;
}
</style>
