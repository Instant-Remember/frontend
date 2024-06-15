<template>
    <div class="navMenu">
        <div class="container">
            <div class="profile" :class="{ active: activeMenuItem === 'profile' }" @click="changeToUser">
                <img src="/src/assets/img/navMenu/profile.svg" alt="">
                <text>Профиль</text>
            </div>
            <div class="feed" :class="{ active: activeMenuItem === 'feed' }" @click="changeToFeed">
                <img src="/src/assets/img/navMenu/feed.svg" alt="">
                <text>Лента</text>
            </div>
            <div class="target" :class="{ active: activeMenuItem === 'goals' }" @click="changeToGoals">
                <img src="/src/assets/img/navMenu/targetG.svg" alt="">
                <text>Цели</text>
            </div>
            <div class="options" :class="{ active: activeMenuItem === 'options' }" @click="changeToOptions">
                <img src="/src/assets/img/navMenu/options.svg" alt="">
                <text>Настройки</text>
            </div>

        </div>
    </div>
</template>

<script>
export default {
    methods: {
        changeToUser() {
            this.$emit('changePage', 'userPage');
            this.activeMenuItem = 'profile';
        },
        changeToGoals() {
            this.$emit('changePage', 'goalsPage');
            this.activeMenuItem = 'goals';
        },
        changeToFeed() {
            this.$emit('changePage', 'feedPage');
            this.activeMenuItem = 'feed';
        },
        changeToOptions() {
            this.$emit('changePage', 'optionsPage');
            this.activeMenuItem = 'options';
        },
    },
    data() {
        return {
            activeMenuItem: 'profile'
        };
    }
}
</script>

<style scoped>
.container {
    width: 121px;
    display: flex;
    flex-direction: column;
    margin-top: 33px;
    margin-left: 38px;
}

.navMenu {
    background-color: #fff9f9;

    height: 193px;
    width: 313px;
    border-radius: 20px;

}

.profile {
    display: flex;
    height: 19px;

    cursor: pointer;


    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    color: #444444;
}



.feed {
    display: flex;
    margin-top: 17px;
    height: 19px;


    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    color: #444444;

    cursor: pointer;

}

.target {
    display: flex;
    height: 19px;
    margin-top: 17px;

    cursor: pointer;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    color: #444444;
}

.options {
    display: flex;
    height: 19px;
    margin-top: 17px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    color: #444444;

    cursor: pointer;
}

text {
    margin-left: 16px;
}

img {
    height: 18px;
    width: 18px;
}

.profile {
    /* ... стили для неактивного состояния ... */
    color: #444444;
    /* Цвет текста по умолчанию */
}

.profile.active {
    color: #6133B4;
    /* Цвет текста для активного состояния */
}

.feed.active {
    color: #6133B4;
    /* Цвет текста для активного состояния */
}

.target.active {
    color: #6133B4;
    /* Цвет текста для активного состояния */
}

.options.active {
    color: #6133B4;
    /* Цвет текста для активного состояния */
}

@media (max-width: 425px) {

    .navMenu {
        height: 80px;
        width: 100vh;
        border-radius: 0;
    }

    .container {
        height: 36px;
        width: 325px;

        display: grid;
        grid-template-columns: 70px 90px 88px 77px;
        grid-template-rows: 35px;
    }

    .profile {
        display: flex;
        flex-direction: column;

        grid-column: 1;
        grid-row: 1;

        width: 53px;
        height: 35px;

        cursor: pointer;


        font-family: 'Inter';
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 19px;
        display: flex;
        align-items: center;

        color: #444444;    
        
        margin-left: 0;
    }



    .feed {
        display: flex;
        flex-direction: column;
        grid-column: 2;
        grid-row: 1;

        margin-top: 0;
        width: 35px;
        height: 35px;


        font-family: 'Inter';
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 19px;
        display: flex;
        align-items: center;

        color: #444444;

        cursor: pointer;

        margin-left: 28px;

    }

    .target {
        display: flex;

        flex-direction: column;
        grid-column: 3;
        grid-row: 1;

        width: 31px;
        height: 35px;
        margin-top: 0;

        cursor: pointer;

        font-family: 'Inter';
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 19px;
        display: flex;
        align-items: center;

        color: #444444;

        margin-left: 28px;
    }

    .options {
        display: flex;
        flex-direction: column;
        grid-column: 4;
        grid-row: 1;

        width: 64px;
        height: 35px;
        margin-top: 0;

        font-family: 'Inter';
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 19px;
        display: flex;
        align-items: center;

        color: #444444;

        cursor: pointer;

        margin-left: 13px;
    }

    text {
        margin-left: 0;
    }

    img {
        height: 18px;
        width: 18px;
    }
}
</style>