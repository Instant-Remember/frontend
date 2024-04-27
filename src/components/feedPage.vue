<template>
    <div class="posts">
        <div class="post" v-for="post in posts" :key="post.id">

            <user class="userpost">
                <img src="/src/assets/img/avatar.svg" alt="">
                <div class="username">{{ users.first_name }} {{ users.last_name }}</div>
            </user>

            <div class="mainpost">
                <div class="targetname">{{ goals.name }}</div>
                <div class="description">{{ post.text }}</div>
            </div>

            <progressBar class="progres"></progressBar>

            <date>Неделю назад</date>
            <society>
                <button class="likes">
                    <img src="/src/assets/img/likes.svg" alt="">
                    <div class="like">{{ countLikes() }}</div>
                </button>
                <button class="comments"><img src="/src/assets/img/comments.svg" alt=""></button>
            </society>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import progressBar from './progressBar.vue'

export default {
    props: {
        backendURL: String
    },

    components: { progressBar },

    data() {
        return {
            posts: [], // Массив для хранения постов
            users: [], // Объект для хранения данных о пользователях
            goals: [], // Объект для хранения данных о целях
            likes: []
        };
    },
    mounted() {
        this.getFeed();
    },
    methods: {
        getFeed() {
            axios.get(`${this.backendURL}/feed?count=5&offset=1`, {
                headers: {
                    'accept': 'application/json',
                    'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6NX0.3-ByMuqeeDCspKySq6ZkqgssryNOPVvE4VWiEEIAtv8'
                }
            })
                .then(response => {
                    this.posts = response.data;
                    this.getUserInfo();
                    this.getGoals();
                    this.getLikes();
                })
                .catch(error => {
                    console.error('Ошибка при получении данных:', error);
                });
        },
        getUserInfo() {
            // Получение данных о пользователе для каждого поста
            this.posts.forEach(post => {
                axios.get(`${this.backendURL}/profile/${post.owner_id}`, {
                    headers: {
                        'accept': 'application/json'
                    }
                })
                    .then(response => {
                        // Обновление данных о пользователе в каждом посте
                        this.users = response.data;
                        console.log('Польхователь:', response.data);
                    })
                    .catch(error => {
                        // Обработка ошибки
                        console.error('Ошибка при получении данных о пользователе:', error);
                    });
            });
        },

        getGoals() {
            this.posts.forEach(post => {
                axios.get(`${this.backendURL}/goal/${post.goal_id}`, {
                    headers: {
                        'accept': 'application/json'
                    }
                })
                    .then(response => {
                        this.goals = response.data;
                        console.log('Цели друга:', response.data);
                    })
                    .catch(error => {
                        console.error('Ошибка при получении данных о цели:', error);
                    });
            });
        },

        getLikes() {
            this.posts.forEach(post => {
                axios.get(`${this.backendURL}/post/${post.id}/likes`, {
                    headers: {
                        'accept': 'application/json'
                    }
                })
                    .then(response => {
                        this.likes = response.data;
                        console.log('Лайки:', response.data);
                    })
                    .catch(error => {
                        console.error('Ошибка при получении лайков:', error);
                    });
            });
        },
        countLikes() {
            return this.likes.length;
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

.userpost {
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