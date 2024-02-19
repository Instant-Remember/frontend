<template>
    <div class="goalsPage">
        <profileHeader></profileHeader>

        <search>
            <searchLine></searchLine>
            <button @click="showPopup = true">Создать цель</button>
        </search>

        <popup v-if="showPopup" @close="showPopup = false">
            <!-- Содержимое всплывающего окна передается через слот -->
            <goalCreate>
                <input type="text" placeholder="Название цели" class="goalName">
                <input type="text" placeholder="Описание..." class="goalDescription">

                <button @click="$emit('close')">Создать</button>
            </goalCreate>

        </popup>

        <section>
            <goal></goal>
            <goal></goal>
            <goal></goal>
            <goal></goal>

        </section>
    </div>
</template>
  
<script>
import searchLine from './searchLine.vue'
import goal from './goal.vue'
import Popup from './Popup.vue'
import profileHeader from './profileHeader.vue'
import progressBar from './progressBar.vue'



export default {
    components: { searchLine, goal, Popup, profileHeader, progressBar },

    data() {
        return {
            showPopup: false
        };
    }
}

</script>
  
<style scoped>
.goalsPage {
    width: 757px;
}

search {

    margin-left: 397px;
    margin-top: 16px;

    width: 756px;
    height: 52px;

    background-color: #fff;

    display: flex;
    align-items: center;

    border-radius: 20px;
}

search button {
    width: 135px;
    height: 41px;

    margin-left: 16px;

    border-radius: 14px;

    border: none;
    outline: none;

    color: #fff;
    background-color: #6133B4;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 17px;

    cursor: pointer;
}

.searchLine {
    margin-left: 16px;

    width: 573px;
}

section {
    width: 757px;

    background-color: #fff;
    border-radius: 20px;

    padding-top: 1px;
    padding-bottom: 16px;

    margin-top: 16px;
    margin-left: 397px;
}

.goalName {

width: 609px;
height: 36px;
margin-top: 15px;
margin-left: 36px;
}

.goalDescription {

width: 609px;
height: 118px;
margin-left: 36px;

}

goalCreate button {
margin-top: 12px;
margin-left: 220px;

width: 246px;
height: 49px;

background: #6133B4;
border-radius: 20px;


border: none;
outline: none;

color: #fff;
background-color: #6133B4;

font-family: 'Inter';
font-style: normal;
font-weight: 400;
font-size: 14px;
line-height: 17px;

cursor: pointer;
}
</style>