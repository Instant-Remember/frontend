<template>
    <div class="goal">

        <targetname>Кибер монстр</targetname>
        <description>Сыграть на про сцене в Counter strike</description>

        
        <progressBar class="progres"></progressBar>

    </div>
</template>

<script>
import progressBar from './progressBar.vue'
export default {
    components:{progressBar}
}
</script>

<style scoped>
.goal {
    height: 45px;
    width: 724px;

    display: grid;
    grid-template-columns: 275px 449px;
    grid-template-rows: 26px 39px;

    margin-left: 16px;
    margin-top: 16px;
  
}

targetname {
    grid-row: 1;
    grid-column: 1;

    height: 19px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    /* Диплом/Акцентный */
    color: #6133B4;
}

description {
    grid-row: 2;
    grid-column: 1 / 2;

    height: 18px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 18px;
    display: flex;
    align-items: center;

    /* Диплом/Текс */
    color: #444444;

    margin-top: 0px;
}

progres {
    grid-row: 1;
    grid-column: 2;

    display: flex;
    align-items: center;
    width: 449px;
    height: 17px;
}

progres img {
    width: 410px;
    height: 7px;

    margin-left: 8px;
}
</style>