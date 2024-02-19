<template>
    <div class="headerPc">
        <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
        <searchLine></searchLine>
        <div class="menu_buttons">
            <p id="p1">Популярное</p>
            <p id="p2">Для тебя</p>
        </div>
        <div class="user">
            <img src="/src/assets/img/avatar.svg" alt="Me">
            <p>Иван Глобиков</p>
        </div>
    </div>
</template>
  
<script>
import searchLine from './searchLine.vue'


export default {
  components: {searchLine}
}
</script>

<style scoped>
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

    width: 1440px;

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
}

.user p {
    margin-top: 12px;
    margin-left: 24px;
    height: 19px;
    font-size: 16px;
}
</style>