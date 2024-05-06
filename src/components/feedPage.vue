<template>
    <div class="posts">
        <div class="post" v-for="post in posts" :key="post.id">

            <user class="userpost">
                <img src="/src/assets/img/avatar.svg" alt="">
                <div class="username">{{ getUserFirstName(post.owner_id) }} {{ getUserLastName(post.owner_id) }}</div>
            </user>

            <div class="mainpost">
                <div class="targetname">{{ getGoalName(post.goal_id) }}</div>
                <div class="description">{{ post.text }}</div>
            </div>

            <progressBar class="progres"></progressBar>

            <date>Неделю назад</date>
            <society>
                <button class="likes" @click="likePost(post.id)">
                    <img src="/src/assets/img/likes.svg" alt="">
                    <div class="like">{{ getLikesCount(post.id) }}</div>
                </button>
                <button class="comments_img"><img src="/src/assets/img/comments.svg" alt=""></button>
            </society>

            <div class="comments">
                <div class="line"></div>
                <div class="comment" v-for="comment in post.comments" :key="comment.id">
                    <img src="/src/assets/img/friends/rodion.svg" alt="" class="avatar">
                    <div class="comment_username">{{ getCommentUserName(comment.user_id) }}</div>
                    <div class="main_comment">{{ comment.text }}</div>
                </div>

                <div class="my_comment">
                    <input type="text" class="write_comment" placeholder="Напишите свой комментарий">
                    <button class="send"><img src="/src/assets/img/send.svg" alt=""></button>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
import progressBar from './progressBar.vue'
import axios from 'axios';
import { reactive, toRefs } from 'vue';

export default {
    components: { progressBar },
    props: {
        backendURL: String
    },
    setup() {
        const state = reactive({
            posts: [],
            users: {},
            goals: {},
            likes: {}
        });

        const getFeed = () => {
            const accessToken = localStorage.getItem('accessToken');

            axios.get(`http://130.193.34.79:8000/feed?count=5&offset=1`, {
                headers: {
                    'accept': 'application/json',
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    state.posts = response.data;

                    state.posts.forEach(post => {
                        getUser(post.owner_id);
                        getGoal(post.goal_id);
                        getLikes(post.id);
                        getComments(post.id);
                    });
                })
                .catch(error => {
                    console.error('Ошибка при получении данных:', error);
                });
        };

        const getUser = (userId) => {
            axios.get(`http://130.193.34.79:8000/profile/${userId}`, {
                headers: {
                    'accept': 'application/json'
                }
            })
                .then(response => {
                    state.users[userId] = response.data;
                })
                .catch(error => {
                    console.error('Ошибка при получении данных о пользователе:', error);
                });
        };

        const getGoal = (goalId) => {
            axios.get(`http://130.193.34.79:8000/goal/${goalId}`, {
                headers: {
                    'accept': 'application/json'
                }
            })
                .then(response => {
                    state.goals[goalId] = response.data;
                })
                .catch(error => {
                    console.error('Ошибка при получении данных о цели:', error);
                });
        };

        const getLikes = (postId) => {
            axios.get(`http://130.193.34.79:8000/post/${postId}/likes`, {
                headers: {
                    'accept': 'application/json'
                }
            })
                .then(response => {
                    state.likes[postId] = response.data;
                })
                .catch(error => {
                    console.error('Ошибка при получении данных о лайках поста:', error);
                });
        };

        const getComments = (postId) => {
            axios.get(`http://130.193.34.79:8000/post/${postId}/comments`, {
                headers: {
                    'accept': 'application/json'
                }
            })
                .then(response => {
                    // Добавляем комментарии к посту
                    state.posts.find(post => post.id === postId).comments = response.data;
                    
                    // Для каждого комментария получаем данные о пользователе
                    response.data.forEach(comment => {
                        getUser(comment.user_id);
                    });
                })
                .catch(error => {
                    console.error('Ошибка при получении данных о комментариях поста:', error);
                });
        };

        const likePost = (postId) => {
            const accessToken = localStorage.getItem('accessToken');

            axios.get(`http://130.193.34.79:8000/post/${postId}/like`, {
                headers: {
                    'accept': 'application/json',
                    'Authorization': `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    getLikes(postId);
                })
                .catch(error => {
                    console.error('Ошибка при отправке лайка:', error);
                });
        };

        const getUserFirstName = (userId) => {
            const user = state.users[userId];
            return user ? user.first_name : '';
        };

        const getUserLastName = (userId) => {
            const user = state.users[userId];
            return user ? user.last_name : '';
        };

        const getGoalName = (goalId) => {
            const goal = state.goals[goalId];
            return goal ? goal.name : '';
        };

        const getLikesCount = (postId) => {
            const likes = state.likes[postId];
            return likes ? likes.length : 0;
        };

        const getUserAvatar = (userId) => {
            const user = state.users[userId];
            return user ? user.avatar : '';
        };

        const getCommentUserName = (userId) => {
            const user = state.users[userId];
            return user ? `${user.first_name} ${user.last_name}` : '';
        };

        return {
            ...toRefs(state),
            getFeed,
            getUser,
            getGoal,
            getLikes,
            getComments,
            likePost,
            getUserFirstName,
            getUserLastName,
            getGoalName,
            getLikesCount,
            getUserAvatar,
            getCommentUserName
        };
    },
    mounted() {
        this.getFeed();
    }
};
</script>

<style scoped>
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
    grid-template-rows: 62px 53px 40px auto;
}

.comments{
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

.line{
    margin-left: -17px;
    height: 10px;
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
    display: flex;
    margin-left: 4px;


    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 22px;

    color: #444444;
}

.my_comment{
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

.send{
    border: none;
    background-color: #fff;
}

</style>
