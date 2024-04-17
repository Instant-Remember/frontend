<template>
    <div class="posts">
        <div class="post" v-for="(post, index) in posts" :key="index">

            <user>
                <img src="/src/assets/img/avatar.svg" alt="">
                <div class="username">{{ user.first_name }} {{ user.last_name }}</div>
            </user>

            <div class="mainpost">
                <div class="targetname">{{ getGoalName(post.goal_id) }}</div>
                <div class="description">{{ post.text }}</div>
            </div>

            <progressBar class="progres"></progressBar>

            <date>Неделю назад</date>
            <society>
                <button class="likes">
                    <img src="/src/assets/img/likes.svg" alt="">
                    <div class="like">1K</div>
                </button>
                <button class="comments"><img src="/src/assets/img/comments.svg" alt=""></button>
            </society>
        </div>
    </div>
</template>

<script>
import progressBar from './progressBar.vue'
import axios from 'axios'

export default {
    components: { progressBar },

    data() {
        return {
            posts: [], // Массив для хранения постов пользователя
            goals: [], // Массив для хранения целей пользователя
            user: {}
        };
    },

    mounted() {
        this.fetchUserPosts(); // Выполнение запроса на получение постов пользователя при загрузке компонента
        this.fetchUserGoals(); // Выполнение запроса на получение целей пользователя при загрузке компонента
        this.fetchUserData(); // Выполнение запроса на получение целей пользователя при загрузке компонента

    },

    methods: {
        fetchUserPosts() {
            const accessToken = localStorage.getItem('accessToken');

            axios.get('http://51.250.111.113:8000/me/posts', {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    console.log('Посты пользователя:', response.data);
                    this.posts = response.data; // Заполнение массива постами из ответа сервера
                })
                .catch(error => {
                    console.error('Ошибка при получении постов:', error);
                });
        },

        fetchUserGoals() {
            const accessToken = localStorage.getItem('accessToken');

            axios.get('http://51.250.111.113:8000/me/goals', {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    console.log('Цели пользователя:', response.data);
                    this.goals = response.data; // Заполнение массива целей из ответа сервера
                })
                .catch(error => {
                    console.error('Ошибка при получении целей:', error);
                });
        },

        getGoalName(goalId) { // Новый метод для получения имени цели по её id
            const goal = this.goals.find(goal => goal.id === goalId);
            return goal ? goal.name : 'Цель не найдена';
        },

        fetchUserData() {
            // Получение токена доступа из локального хранилища
            const accessToken = localStorage.getItem('accessToken');

            // Выполнение запроса к серверу с токеном доступа
            axios.get('http://51.250.111.113:8000/me', {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    // Обработка успешного ответа
                    console.log('Информация о пользователе:', response.data);
                    this.user = response.data;
                })
                .catch(error => {
                    // Обработка ошибки
                    console.error('Ошибка:', error);
                    this.error = error.message;
                });
        }
    }
}
</script>

<style scoped>
.post {
    width: 757px;
    height: auto;
    margin-left: 397px;
    margin-top: 16px;

    border-radius: 20px;
    background-color: #fff;

    display: grid;
    grid-template-columns: 69px 555px 133px;
    grid-template-rows: 62px 53px 40px;
}

user {
    grid-row: 1;
    grid-column: 1 / 2;

    display: flex;

    width: 624px;

}

user img {
    width: 42px;
    height: 42px;

    margin: 16px 16px;
}

.username {
    margin-top: 27px;
    width: 118px;
    height: 19px;


    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    color: #444444;
    width: 550px;

}

.mainpost {
    grid-row: 2;
    grid-column: 2 / 3;

    margin-left: 5px;
}

.targetname {

    width: 95px;
    height: 18px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 600;
    font-size: 15px;
    line-height: 18px;
    display: flex;
    align-items: center;
    text-align: center;

    /* Диплом/Акцентный */
    color: #6133B4;


    /* Inside auto layout */
    flex: none;
    order: 0;
    flex-grow: 0;

}

.description {

    width: 608px;
    height: auto;

    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 22px;

    color: #444444;

    margin-top: 4px;
}

.progres {
    grid-row: 3;
    grid-column: 1 / 2;

    display: flex;

    width: 533px;
}


date {
    grid-row: 1;
    grid-column: 3;

    margin-top: 27px;
    margin-left: 7px;

    width: 109px;
    height: 19px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 200;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    color: #444444;
}

society {
    grid-row: 3;
    grid-column: 3;

    display: flex;

    margin-left: 34px;
    margin-top: 3px;
}

.likes {
    display: flex;
    width: 40px;
    height: 19px;

    padding: 0;
    background-color: #fff;
    border: none;
    cursor: pointer;
}

.like {
    margin-left: 4px;

    width: 18px;
    height: 18px;

    font-family: 'Roboto';
    font-style: normal;
    font-weight: 200;
    font-size: 15px;
    line-height: 18px;

    display: flex;
    align-items: center;

    color: #444444;


}

.comments {
    width: 18px;
    height: 18px;
    margin-left: 24px;
    padding: 0;
    background-color: #fff;
    border: none;
    cursor: pointer;
}
</style>