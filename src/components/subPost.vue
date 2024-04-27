<template>
    <div class="posts">
        <div class="post">

            <user>
                <img src="/src/assets/img/avatar.svg" alt="">
                <div class="username">тест</div>
            </user>

            <div class="mainpost">
                <div class="targetname">тест</div>
                <div class="description">тест</div>
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
    props: {
        backendURL: String
    },

    components: { progressBar },

    data() {
        return {
            subscriptions: [],
            posts: [],
            userProfile: {}, // Добавляем объект для хранения информации о пользователе
            userGoals: [], // Инициализируем массив для хранения данных о целях пользователя
            userProfiles: [] // Инициализируем массив для хранения данных о пользователях
        };
    },

    mounted() {
        this.fetchSubscriptions();
    },

    methods: {
        fetchSubscriptions() {
            const accessToken = localStorage.getItem('accessToken');
            axios.get(`${this.backendURL}/me/subscriptions`, {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    console.log('Подписки:', response.data);
                    this.subscriptions = response.data;
                    this.fetchPostsForSubscriptions();
                    this.fetchGoalsForSubscriptions();
                    this.fetchUserProfiles(); // Получаем информацию о пользователях
                })
                .catch(error => {
                    console.error('Ошибка при получении подписок:', error);
                });
        },

        fetchPostsForSubscriptions() {
            const accessToken = localStorage.getItem('accessToken');
            this.subscriptions.forEach(subscription => {
                const publisherId = subscription.publisher_id;
                axios.get(`${this.backendURL}/profile/${publisherId}/posts`, {
                    headers: {
                        Authorization: `Bearer ${accessToken}`
                    }
                })
                    .then(response => {
                        console.log(`Посты пользователя с ID ${publisherId}:`, response.data);
                        this.posts = this.posts.concat(response.data);
                    })
                    .catch(error => {
                        console.error(`Ошибка при получении постов пользователя с ID ${publisherId}:`, error);
                    });
            });
        },

        fetchGoalsForSubscriptions() {
            const accessToken = localStorage.getItem('accessToken');
            this.subscriptions.forEach(subscription => {
                const publisherId = subscription.publisher_id;
                axios.get(`${this.backendURL}/profile/${publisherId}/goals?user_id=${publisherId}`, {
                    headers: {
                        Authorization: `Bearer ${accessToken}`
                    }
                })
                    .then(response => {
                        console.log(`Цели пользователя с ID ${publisherId}:`, response.data);
                        // Сохраняем цели пользователя
                        this.userGoals.push(response.data);
                    })
                    .catch(error => {
                        console.error(`Ошибка при получении целей пользователя с ID ${publisherId}:`, error);
                    });
            });
        },

        fetchUserProfiles() {
            const accessToken = localStorage.getItem('accessToken');
            this.subscriptions.forEach(subscription => {
                const publisherId = subscription.publisher_id;
                axios.get(`${this.backendURL}/profile/${publisherId}`, {
                    headers: {
                        Authorization: `Bearer ${accessToken}`
                    }
                })
                    .then(response => {
                        console.log(`Информация о пользователе с ID ${publisherId}:`, response.data);
                        // Сохраняем информацию о пользователе
                        this.userProfiles.push(response.data);
                    })
                    .catch(error => {
                        console.error(`Ошибка при получении информации о пользователе с ID ${publisherId}:`, error);
                    });
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

    width: auto;
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
    margin-top: 15px;
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