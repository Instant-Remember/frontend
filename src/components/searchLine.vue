<template>
    <div class="searchLine">
        <img src="/src/assets/img/search.svg" alt="">
        <input type="text" placeholder="Поиск">
    </div>
</template>
  
<script>
export default {

}
</script>
  
<style scoped>
.searchLine input {
    width: 77%;
    background-color: #E0E6EF;
    border: none;
    padding-left: 12px;

    font-size: 14px;
    outline: none;
}

.searchLine img {
    height: 20px;
    width: 20px;

    margin-top: 9px;
    margin-left: 12px;
}

.searchLine{
    height: 41px;
    display: flex;
    background-color: #E0E6EF;

    border-radius: 20px;
}
</style>