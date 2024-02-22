<template>
  <div class="friendsMenu">
    <friend1 @click="openFriendPage('Andrey')">
      <img src="/src/assets/img/friends/andreyK.svg" alt="">
      <text>Андрей</text>
    </friend1>

    <friend2 @click="openFriendPage('Andrey')">
      <img src="/src/assets/img/friends/andrey.svg" alt="">
      <text>Андрей</text>
    </friend2>

    <friend3 @click="openFriendPage('Vladislav')">
      <img src="/src/assets/img/friends/vladislav.svg" alt="">
      <text>Владислав</text>
    </friend3>

    <friend4 @click="openFriendPage('Rodion')">
      <img src="/src/assets/img/friends/rodion.svg" alt="">
      <text>Родион</text>
    </friend4>
  </div>
</template>

<script>
export default {
  methods: {
    openFriendPage(friendName) {
      this.$emit('changePage', 'FriendPage', { friendName });
    },
    // Другие методы для других друзей
  }
}
</script>

<style scoped>
.friendsMenu {
  width: 202px;
  height: 261px;
  background-color: #fff9f9;
  border-radius: 20px;

  padding-left: 31px;

  justify-content: center;

  display: flex;
  flex-direction: column;
}

friend1 {
  display: flex;
  align-items: center;
  margin-top: 0px;
  height: 36px;

  cursor: pointer;
}

friend2 {
  display: flex;
  align-items: center;
  height: 36px;
  margin-top: 24px;

  cursor: pointer;
}

friend3 {
  display: flex;
  align-items: center;
  height: 36px;
  margin-top: 24px;

  cursor: pointer;
}

friend4 {
  display: flex;
  align-items: center;
  height: 36px;
  margin-top: 24px;

  cursor: pointer;
}

img {
  width: 36px;
  height: 36px;
}

text {
  margin-left: 24px;
}</style>
