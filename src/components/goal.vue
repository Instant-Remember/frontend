<template>
    <div class="goals">
        <div v-for="(goal, index) in goals" :key="index" class="goal">
            <div class="targetname">{{ goal.name }}</div>
            <div class="description">{{ goal.description }}</div>
            <progressBar class="progres"></progressBar>
        </div>
    </div>
</template>

<script>
import progressBar from './progressBar.vue'
import axios from 'axios';

export default {
    props: {
        backendURL: String
    },

    components: { progressBar },

    data() {
        return {
            goals: [] // Массив для хранения целей пользователя
        };
    },

    mounted() {
        this.fetchUserGoals(); // Выполнение запроса на получение целей пользователя при загрузке компонента
    },

    methods: {
        fetchUserGoals() {
            const accessToken = localStorage.getItem('accessToken');

            axios.get(`${this.backendURL}/me/goals`, {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    this.goals = response.data; // Заполнение массива целями из ответа сервера
                })
                .catch(error => {
                    console.error('Ошибка при получении целей:', error);
                });
        }
    }
}
</script>

<style scoped>
.goals {
    width: 757px;

    background-color: #fff;
    border-radius: 20px;

    padding-top: 1px;
    padding-bottom: 16px;
}

.goal {
    width: 724px;
    height: auto;

    display: grid;
    grid-template-columns: 275px 449px;
    grid-template-rows: 26px auto;

    margin-left: 16px;
    margin-top: 16px;

}

.targetname {
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

.description {
    grid-row: 2;
    grid-column: 1 / 2;

    width: 724px;
    height: auto;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 18px;
  
    align-items: center;

    color: #444444;

    margin-top: 25px;

    word-wrap: break-word;
}

.progres {
    grid-row: 1;
    grid-column: 2;

    display: flex;
    align-items: center;
    width: 449px;
    height: 17px;
}

.progres img {
    width: 410px;
    height: 7px;

    margin-left: 8px;
}

@media (max-width: 425px) {
    .goals {
        width: 375px;
        background-color: none;
    }

    .goal {
        width: 311px;
        height: auto;

        display: grid;
        grid-template-columns: 132px 211px;
        grid-template-rows: 42px auto;
    }

    .targetname {
        margin-top: 0px;
        margin-left: 0px;

    }

    .description {
        width: 311px;
        margin-top: 0px;
    }

    .progres {
        grid-row: 1;
        grid-column: 2;

        display: flex;
        align-items: center;
        width: 189px;
        height: 17px;
    }

    .progres img {
        width: 410px;
        height: 7px;

        margin-left: 8px;
    }
}
</style>
