<template>
  <body>
    <header>
      <component :is="headerComponent"></component>
    </header>
    <aside v-if="!isAuthWindowOpen">
      <navMenu @changePage="changePage"></navMenu>
    </aside>
    <friendsMenu v-if="!isAuthWindowOpen" class="friendsMenu"></friendsMenu>
    <section class="main">
      <component :is="currentPage" @loginSuccess="loginSuccess" @registerSuccess="registerSuccess" @goToRegisterPage="goToRegisterPage" @goToAuthPage="goToAuthPage" />
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
import HeaderPcAuth from './components/headerPcAuth.vue'
import AuthPage from './components/AuthPage.vue'
import RegisterPage from './components/RegisterPage.vue'

export default {
  components: { headerPc, navMenu, friendsMenu, headerPcAuth, userPage, goalsPage, feedPage, optionsPage, HeaderPcAuth, AuthPage, RegisterPage },
  
  data(){
    return{
      currentPage: 'AuthPage',
      isAuthenticated: false,
      isRegisterMode: false,
      isAuthWindowOpen: true,
    };
  },
  computed: {
    headerComponent() {
      return this.isAuthenticated ? 'headerPc' : 'headerPcAuth';
    },
  },
  methods:{
    changePage(page){
      if (this.isAuthWindowOpen) return;
      if (!this.isAuthenticated && !this.isRegisterMode) {
        this.currentPage = 'AuthPage';
      } else {
        this.currentPage = page;
      }
    },
    loginSuccess() {
      this.isAuthenticated = true;
      this.isAuthWindowOpen = false;
      this.currentPage = 'userPage'; // Или другое имя компонента, отвечающего за профиль
    },
    registerSuccess() {
      this.isRegisterMode = false;
      // Дополнительная логика после успешной регистрации
      this.loginSuccess(); // Вызываем метод для корректного отображения после регистрации
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
    },
    goToRegisterPage() {
      this.isRegisterMode = true;
      this.currentPage = 'RegisterPage';
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
</style>
