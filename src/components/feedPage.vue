<template>
    <div class="posts">
        <div class="post" v-for="post in posts" :key="post.id">

            <user class="userpost">
                <img :src="getUserAvatar(post.owner_id)" alt="" class="avatar_post">
                <div class="username">{{ getUserFirstName(post.owner_id) }} {{ getUserLastName(post.owner_id) }}</div>
            </user>

            <div class="mainpost">
                <div class="targetname">{{ getGoalName(post.goal_id) }}</div>
                <div class="description">{{ post.text }}</div>
            </div>

            <progressBar class="progres" :progress="post.progress"></progressBar>

            <date>{{ formatDate(post.date_create) }}</date>
            <society>
                <button class="likes" @click="likePost(post.id)">
                    <img src="/src/assets/img/likes.svg" alt="">
                    <div class="like">{{ getLikesCount(post.id) }}</div>
                </button>
                <button class="comments_img"><img src="/src/assets/img/comments.svg" alt=""></button>
            </society>

            <div class="comments">
                <div class="line"></div>
                <div class="container">
                    <div class="comment" v-for="comment in post.comments" :key="comment.id">
                        <img :src="getUserAvatar(comment.user_id)" alt="" class="avatar">
                        <div class="comment_username">{{ getCommentUserName(comment.user_id) }}</div>
                        <div class="main_comment">{{ comment.text }}</div>
                    </div>
                </div>

                <div class="my_comment">
                    <input type="text" class="write_comment" placeholder="Напишите свой комментарий"
                        v-model="newCommentText">
                    <button type="button" @click="addComment(post.id)" class="send"><img src="/src/assets/img/send.svg"
                            alt=""></button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import progressBar from './progressBar.vue'
import axios from 'axios';

export default {
    components: { progressBar },
    props: {
        backendURL: String
    },
    data() {
        return {
            posts: [],
            users: {},
            goals: {},
            likes: {},
            newCommentText: ''
        };
    },
    methods: {
        formatDate(dateString) {
            const months = [
                "января", "февраля", "марта", "апреля", "мая", "июня",
                "июля", "августа", "сентября", "октября", "ноября", "декабря"
            ];

            const dateObj = new Date(dateString);
            const day = dateObj.getDate();
            const monthIndex = dateObj.getMonth();
            const year = dateObj.getFullYear();

            return `${day} ${months[monthIndex]} ${year}`;
        },
        getFeed() {
            const accessToken = localStorage.getItem('accessToken');

            axios.get(`${this.backendURL}/feed?count=5&offset=1`, {
                headers: {
                    'accept': 'application/json',
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    this.posts = response.data;

                    this.posts.forEach(post => {
                        this.getUser(post.owner_id);
                        this.getGoal(post.goal_id);
                        this.getLikes(post.id);
                        this.getComments(post.id);
                    });
                })
                .catch(error => {
                    console.error('Ошибка при получении данных:', error);
                });
        },
        getUser(userId) {
            axios.get(`${this.backendURL}/profile/${userId}`, {
                headers: {
                    'accept': 'application/json'
                }
            })
                .then(response => {
                    this.users[userId] = response.data;

                    this.users[userId].avatar = response.data.profile_photo;
                })
                .catch(error => {
                    console.error('Ошибка при получении данных о пользователе:', error);
                });
        },
        getUserAvatar(userId) {
            const user = this.users[userId];
            return user && user.profile_photo ? user.profile_photo : '/src/assets/img/avatar.svg'; // Если ссылка на аватар отсутствует, используем заглушку
        },
        getGoal(goalId) {
            axios.get(`${this.backendURL}/goal/${goalId}`, {
                headers: {
                    'accept': 'application/json'
                }
            })
                .then(response => {
                    this.goals[goalId] = response.data;
                })
                .catch(error => {
                    console.error('Ошибка при получении данных о цели:', error);
                });
        },
        getLikes(postId) {
            axios.get(`${this.backendURL}/post/${postId}/likes`, {
                headers: {
                    'accept': 'application/json'
                }
            })
                .then(response => {
                    this.likes[postId] = response.data;
                })
                .catch(error => {
                    console.error('Ошибка при получении данных о лайках поста:', error);
                });
        },
        getComments(postId) {
            axios.get(`${this.backendURL}/post/${postId}/comments`, {
                headers: {
                    'accept': 'application/json'
                }
            })
                .then(response => {
                    this.posts.find(post => post.id === postId).comments = response.data;

                    // Для каждого комментария получаем данные о пользователе
                    response.data.forEach(comment => {
                        this.getUser(comment.user_id);
                    });
                })
                .catch(error => {
                    console.error('Ошибка при получении данных о комментариях поста:', error);
                });
        },
        likePost(postId) {
            const accessToken = localStorage.getItem('accessToken');

            axios.get(`${this.backendURL}/post/${postId}/like`, {
                headers: {
                    'accept': 'application/json',
                    'Authorization': `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    this.getLikes(postId);
                })
                .catch(error => {
                    console.error('Ошибка при отправке лайка:', error);
                });
        },
        getUserFirstName(userId) {
            const user = this.users[userId];
            return user ? user.first_name : '';
        },
        getUserLastName(userId) {
            const user = this.users[userId];
            return user ? user.last_name : '';
        },
        getGoalName(goalId) {
            const goal = this.goals[goalId];
            return goal ? goal.name : '';
        },
        getLikesCount(postId) {
            const likes = this.likes[postId];
            return likes ? likes.length : 0;
        },
        getCommentUserName(userId) {
            const user = this.users[userId];
            return user ? `${user.first_name} ${user.last_name}` : '';
        },
        addComment(postId) {
            const accessToken = localStorage.getItem('accessToken');
            const newCommentText = this.newCommentText.trim();

            if (newCommentText) {
                axios.post(`${this.backendURL}/comment/create`, {
                    text: newCommentText,
                    post_id: postId
                }, {
                    headers: {
                        'accept': 'application/json',
                        'Authorization': `Bearer ${accessToken}`,
                        'Content-Type': 'application/json'
                    }
                })
                    .then(response => {
                        // Опционально: обновление списка комментариев
                        this.getComments(postId);
                        // Очистка поля ввода после успешной отправки
                        this.newCommentText = '';
                    })
                    .catch(error => {
                        console.error('Ошибка при добавлении комментария:', error);
                    });
            }
        }
    },
    mounted() {
        this.getFeed();
    }
};

</script>


<style scoped>
.avatar_post{
    border-radius: 20px;
}

.container {
    max-height: 170px;
    width: 757px;
    overflow-y: auto;
    /* Добавляем вертикальную прокрутку */
    scrollbar-width: none;
}

.posts {
    margin-top: -15px;
}

.post {
    width: 757px;
    height: auto;
    margin-left: 397px;
    margin-top: 16px;

    border-radius: 20px;
    background-color: #fff;

    display: grid;
    grid-template-columns: 69px 555px 133px;
    grid-template-rows: 62px auto 40px auto;
}

.comments {
    grid-row: 4;
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

    word-wrap: break-word;
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

.comments_img {
    width: 18px;
    height: 18px;
    margin-left: 24px;
    padding: 0;
    background-color: #fff;
    border: none;
    cursor: pointer;
}

.line {
    height: 5px;
    width: 757px;

    grid-row: 1;
    grid-column: 1 / 2;
    background-color: #E8ECF2;
}

.comment {
    grid-row: 1;
    grid-column: 1;

    grid-row: 1;
    display: grid;
    grid-template-columns: 41px 645px;
    grid-template-rows: 40px auto;

}

.avatar {
    width: 28px;
    height: 28px;
    grid-row: 1;
    grid-column: 1;
    margin-top: 12px;
    margin-left: 11px;
    border-radius: 20px;
}

.comment_username {
    grid-row: 1;
    grid-column: 2;

    margin-top: 12px;
    margin-left: 5px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 18px;
    display: flex;
    align-items: center;

    color: #444444;
}

.main_comment {
    grid-row: 2;
    grid-column: 2;

    margin-left: 4px;


    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 22px;

    color: #444444;
    word-wrap: break-word;
}

.my_comment {
    grid-row: 2;
    grid-column: 1;

    width: 687px;
    display: flex;
    margin-left: 19px;

    margin-top: 5px;
}

.write_comment {


    display: flex;


    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 18px;
    display: flex;
    align-items: center;

    color: #8C8C8C;
}

.send {
    border: none;
    background-color: #fff;
}

@media (max-width: 425px) {
    .posts {
        width: 375px;

        margin-left: 0px;
        margin-top: 100px;

        padding-bottom: 100px
    }

    .post {
        width: 343px;
        margin-left: 16px;

        display: grid;
        grid-template-columns: 223px 120px;
        grid-template-rows: 62px auto 33px auto;
    }

    .userpost {
        grid-row: 1;
        grid-column: 1;

        display: flex;

        width: fit-content;

    }

    .avatar {
        width: 42px;
        height: 42px;
        border-radius: 65px;
    }

    .username {
        margin-top: 27px;

        height: 19px;


        font-family: 'Inter';
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 19px;
        display: flex;
        align-items: center;

        color: #444444;
        width: fit-content;

    }

    .mainpost {
        grid-row: 2;
        grid-column: 1 / 2;

        margin-left: 16px;
    }


    .description {
        width: 311px;
        height: auto;
        margin-top: 4px;
        word-wrap: break-word;
    }

    .progres {
        grid-row: 3;
        grid-column: 1;

        width: 250px;
        margin-top: 0px;
        margin-left: -15px;
    }


    date {
        grid-row: 1;
        grid-column: 2;

        margin-top: 27px;
    }

    society {
        grid-row: 3;
        grid-column: 2;

    }

    .container {
        max-height: 120px;
        width: 343px;

        scrollbar-width: none !important;


    }

    .comments {
        grid-row: 4;
    }

    .comments_img {
        width: 18px;
        height: 18px;
        margin-left: 24px;
        padding: 0;
        background-color: #fff;
        border: none;
        cursor: pointer;
    }

    .line {
        width: 343px;

        grid-row: 1;
        grid-column: 1 / 2;
    }



    .avatar {
        width: 28px;
        height: 28px;
        grid-row: 1;
        grid-column: 1;
        margin-top: 12px;
        margin-left: 11px;
    }

    .my_comment {
        grid-row: 2;
        grid-column: 1;

        width: 343px;
        display: flex;
        margin-left: 19px;

        margin-top: 5px;

        margin-left: 0px;
    }

    .write_comment {
        display: flex;

        width: 250px;

        margin-left: 0;
    }

    .send {
        border: none;
        background-color: #fff;
    }


    .comment_username {
        width: fit-content;
    }

    .main_comment {
        width: 280px;
    }
}
</style>
