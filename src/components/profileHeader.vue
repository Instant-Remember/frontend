<template>
    <div class="profileHeader">

        <photo>
            <img src="/src/assets/img/avatar.svg" alt="">
        </photo>

        <description>
            <user>
                <div class="test1">Иван Глобиков</div>
                <div class="test2">миллионер</div>
            </user>
            <target>
                <div>5</div>
                <img src="/src/assets/img/navMenu/targetV.svg" alt="">
            </target>

            <div class="nametag">@ivang</div>

            <subs>
                <div class="subscribers">10к Подписчиков</div>
                <div class="subscribe">100 Подписок</div>
            </subs>

            <button>Редактировать</button>

        </description>
    </div>
</template>

<script>
export default{

}
</script>

<style scoped>
.profileHeader {
    width: 757px;
    height: 193px;
    margin-left: 397px;

    background-image: url("/src/assets/img/profileHeader.svg");
    border-radius: 20px;

    display: flex;
}

description {

    display: grid;
    grid-template-columns: 157px 133px 123px 159px;
    grid-template-rows: 45px 18px;

    width: 548px;
    height: 63px;

    margin-left: 24px;
    margin-top: 115px;

}

photo {
    width: 130px;
    height: 130px;
    border-radius: 60px;

    background-color: #F6F1F1;

    margin-top: 46px;
    margin-left: 31px;
}

user {
    width: 119px;
    height: 39px;

    grid-row: 1;
    grid-column: 1;
}

.test1 {
    font-family: 'Inter';
    font-style: medium;
    font-weight: 500;
    font-size: 16px;
    line-height: 19px;

    color: #444444;
}

.test2 {
    font-family: 'Inter';
    font-style: normal;
    font-weight: 300;
    font-size: 14px;
    line-height: 17px;

    color: #444444;
    margin-top: 3px;
}

target {

    width: 54px;
    height: 36px;

    display: flex;

    grid-row: 1;
    grid-column: 2;
}

target div {

    font-family: 'Jost';
    font-style: normal;
    font-weight: 200;
    font-size: 24px;
    line-height: 35px;
    display: flex;
    align-items: center;

    color: #444444;

}

target img {
    margin-left: 4px;

    width: 36px;
    height: 36px;
}

.nametag {
    font-family: 'Comfortaa';
    font-style: normal;
    font-weight: 300;
    font-size: 16px;
    line-height: 18px;

    display: flex;
    align-items: center;

    color: #004ABA;

    grid-row: 1;
    grid-column: 3;
}

description button {
    grid-row: 1 / 2;
    grid-column: 4;
}

subs {
    grid-row: 2;
    grid-column: 2 / 3;

    display: flex;

    width: 219px;
    height: 16px;

    /* Group 336494 */

    font-family: 'Inter';
    font-style: normal;
    font-weight: 300;
    font-size: 13px;
    line-height: 16px;

    color: #8C8C8C;
}

.subscribe {
    margin-left: 24px;
}

button{
    width: 135px;
    height: 41px;

    border-radius: 20px;

    border-color: #6133B4;
    color: #6133B4;

    outline: none;

    margin-top: 14px;
    cursor: pointer;
}


</style>