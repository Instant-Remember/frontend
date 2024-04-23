<template>
    <div class="profileHeader">

        <photo>
            <img src="/src/assets/img/avatar.svg" alt="">
        </photo>

        <description>
            <user>
                <div class="test1">{{ user.first_name }} {{ user.last_name }}</div>
                <div class="test2">{{ user.about }}</div>
            </user>
            <target>
                <div>{{ countGoals() }}</div>
                <img src="/src/assets/img/navMenu/targetV.svg" alt="">
            </target>

            <div class="nametag">@{{ user.username }}</div>

            <subs>
                <div class="subscribers">{{ countSubscribers() }} подписчиков</div>
                <div class="subscribe">{{ countSubscriptions() }} Подписок</div>
            </subs>

            <button>Редактировать</button>

        </description>
    </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      user: {},
      goals:[],
      subscribers: [],
      subscriptions: []
    };
  },
  created() {
    this.fetchUserData();
    this.fetchUserGoals();
    this.fetchUserSubscribers();
    this.fetchUserSubscriptions();
  },
  methods: {
    fetchUserData() {
      // Получение токена доступа из локального хранилища
      const accessToken = localStorage.getItem('accessToken');

      // Выполнение запроса к серверу с токеном доступа
      axios.get('http://158.160.88.115:8000/me', {
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
    },

    fetchUserGoals() {
            const accessToken = localStorage.getItem('accessToken');

            axios.get('http://158.160.88.115:8000/me/goals', {
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
        countGoals() { // Метод для подсчета количества целей пользователя
            return this.goals.length;
        },

    fetchUserSubscribers(){
        const accessToken = localStorage.getItem('accessToken');

            axios.get('http://158.160.88.115:8000/me/subscribers', {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    console.log('Подписчики:', response.data);
                    this.subscribers = response.data; // Заполнение массива целей из ответа сервера
                })
                .catch(error => {
                    console.error('Ошибка при получении подписчиков:', error);
                });
    },
    countSubscribers() { // Метод для подсчета количества целей пользователя
            return this.subscribers.length;
        },

        fetchUserSubscriptions(){
        const accessToken = localStorage.getItem('accessToken');

            axios.get('http://158.160.88.115:8000/me/subscriptions', {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    console.log('Подписки:', response.data);
                    this.subscriptions = response.data; // Заполнение массива целей из ответа сервера
                })
                .catch(error => {
                    console.error('Ошибка при получении подписок:', error);
                });
    },
    countSubscriptions() { // Метод для подсчета количества целей пользователя
            return this.subscriptions.length;
        },
  }
};
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

    height: 19px;
    width: 550px;

    margin-top: 10px;
}

.test2 {
    font-family: 'Inter';
    font-style: normal;
    font-weight: 300;
    font-size: 14px;
    line-height: 17px;

    color: #444444;
    margin-top: 8px;
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

    height: 18px;
    margin-top: 16px;
    margin-left: -18px;
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