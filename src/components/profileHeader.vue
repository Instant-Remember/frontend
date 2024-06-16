<template>
    <div class="profileHeader">
        <photo @click="openFileInput">
            <img :src="user.profile_photo" alt="Photo" v-if="user.profile_photo" class="avatar">
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
                <div class="subscribe" @click="handleSubscribeClick">{{ countSubscriptions() }} подписок</div>
            </subs>

            <button>Редактировать</button>
        </description>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    props: {
        backendURL: String
    },

    data() {
        return {
            user: {},
            goals: [],
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
            const accessToken = localStorage.getItem('accessToken');
            axios.get(`${this.backendURL}/me`, {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    this.user = response.data;
                })
                .catch(error => {
                    console.error('Ошибка:', error);
                    this.error = error.message;
                });
        },

        fetchUserGoals() {
            const accessToken = localStorage.getItem('accessToken');
            axios.get(`${this.backendURL}/me/goals`, {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    this.goals = response.data;
                })
                .catch(error => {
                    console.error('Ошибка при получении целей:', error);
                });
        },
        countGoals() {
            return this.goals.length;
        },

        fetchUserSubscribers() {
            const accessToken = localStorage.getItem('accessToken');
            axios.get(`${this.backendURL}/me/subscribers`, {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    this.subscribers = response.data;
                })
                .catch(error => {
                    console.error('Ошибка при получении подписчиков:', error);
                });
        },
        countSubscribers() {
            return this.subscribers.length;
        },

        fetchUserSubscriptions() {
            const accessToken = localStorage.getItem('accessToken');
            axios.get(`${this.backendURL}/me/subscriptions`, {
                headers: {
                    Authorization: `Bearer ${accessToken}`
                }
            })
                .then(response => {
                    this.subscriptions = response.data;
                })
                .catch(error => {
                    console.error('Ошибка при получении подписок:', error);
                });
        },
        countSubscriptions() {
            return this.subscriptions.length;
        },

        handleSubscribeClick() {
            this.$emit('toggleFriendsMenu');
        },
        toggleFriendsMenu() {
            console.log('Subscribe button clicked');
            this.$emit('toggleFriendsMenu');
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

.avatar {
    width: 130px;
    height: 130px;
    border-radius: 65px;
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

    margin-left: -25px;
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

button {
    width: 135px;
    height: 41px;

    border-radius: 20px;

    border-color: #6133B4;
    color: #6133B4;

    outline: none;

    margin-top: 14px;
    cursor: pointer;
}

@media (max-width: 425px) {
    .profileHeader {
        width: 375px;
        height: 319px;

        margin-left: 0px;
        margin-top: 135px;

        background-image: url("/src/assets/img/profileHeaderMob.svg");


    }


    photo {
        margin-left: 123px;
        margin-top: 50px;

    }

    description {
        width: 375px;
        height: 207px;

        margin-left: -253px;
        margin-top: 111px;

        display: grid;
        grid-template-columns: 122px 131px 122px;
        grid-template-rows: 71px 47px 35px 54px;
    }

    user {
        grid-column: 2;
        grid-row: 2;

        width: 253px;
        height: 47px;
        display: flex;
        flex-direction: column;

        margin-top: 5px;
    }

    .test1 {
        margin-top: 0;
        margin-left: 0;
        width: fit-content;
    }

    .test2 {
        margin-top: 2px;
        margin-left: -120px;

        text-align: center;
        width: 370px;

    }

    target {
        grid-column: 1;
        grid-row: 1;

        width: 122px;
        height: 49px;
        margin-top: 9px;

        display: flex;
        align-items: center;
        justify-content: center;
    }

    target div {
        margin-top: 0;
    }

    target img {
        margin-top: 0;
    }

    .nametag {
        grid-column: 3;
        grid-row: 1;

        margin-top: 22px;
        margin-left: 22px;
    }

    description button {
        grid-column: 1 2 3;
        grid-row: 4;

        margin-left: -340px;
        width: 310px;
        margin-top: 0px;
    }

    subs {
        grid-column: 1 2 3;
        grid-row: 3;

        margin-top: 4px;

        width: 375px;
        margin-left: -120px;
    }

    .subscribers {
        margin-left: 16px;
    }

    .subscribe {
        margin-left: 190px;
    }

}
</style>