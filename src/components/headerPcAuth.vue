<template>
    <div className ="headerPcAuth">
        <img src="/src/assets/img/logo.svg" alt="Лого" class="logo">
    </div>
</template>
  
<script>
export default {
   
}
</script>

<style scoped>
* {
    display: flex;
}

.headerPcAuth {
    width: 1440px;
    height: 70px;
    background-color: #fff;
    backdrop-filter: blur(16px);
    display: flex;

    margin-left: auto;
    margin-right: auto;

    align-items: center;
}

.logo {
    margin-top: 11px;
    margin-left: 64px;
}

</style>