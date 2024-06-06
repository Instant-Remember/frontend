<template>
    <div class="userPage">
      <profileHeader :backendURL="backendURL" @toggleFriendsMenu="toggleFriendsMenu"></profileHeader>
      <friendsMenu v-if="showFriendsMenu" :backendURL="backendURL" @friendSelected="friendSelected" class="friendsMenu"></friendsMenu>
      <postEditor :backendURL="backendURL"></postEditor>
      <post :backendURL="backendURL"></post>
    </div>
  </template>
  
  <script>
  import post from './post.vue'
  import postEditor from './postEditor.vue'
  import profileHeader from './profileHeader.vue'
  import friendsMenu from '../components/friendsMenu.vue'
  
  export default {
    components: { postEditor, post, profileHeader, friendsMenu },
    props: {
      backendURL: {
        type: String,
        required: true
      }
    },
    data() {
      return {
        showFriendsMenu: false
      };
    },
    methods: {
      toggleFriendsMenu() {
        this.showFriendsMenu = !this.showFriendsMenu;
        this.$emit('toggleFriendsMenu');
      },
      friendSelected(id) {
        this.$emit('friendSelected', id);
      }
    }
  };
  </script>
  

<style scoped>
@media(min-width:768px) {
    .friendsMenu {
        display: none;
    }
}

@media(max-width: 375px) {
    .userPage {
        height: calc(100vh + 550px);
    }

    .friendsMenu {
        margin-top: 16px;
    }
}
</style>
