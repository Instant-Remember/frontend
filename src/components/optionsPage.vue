<template>
    <div class="options">
        <div class="profileHeader">
            <photo @click="openFileInput">
                <img :src="user.profile_photo" alt="Photo" v-if="user.profile_photo" class="avatar">
            </photo>
            <input type="file" id="avatarInput" style="display: none" @change="handleFileChange">
            <bgEditor>
                <img src="/src/assets/img/pencil.svg" alt="">
                <p>Изменить</p>
            </bgEditor>
            <name>
                <input type="text" placeholder="Иван Глобиков" class="username" v-model="newFirstName">
                <div class="linee"></div>
                <input type="text" placeholder="@ivang" class="nametag" v-model="newUsername">
            </name>
        </div>

        <div class="editor">
            <description>
                <p>Описание:</p>
                <input type="text" placeholder="миллионер" v-model="newAbout">
            </description>

            <div class="line"></div>

            <profile>
                <label for="closeProfile">Закрытый профиль:</label>
                <select v-model="selectedType" id="closeProfile">
                    <option value="no">Нет</option>
                    <option value="yes">Да</option>
                </select>
            </profile>

            <div class="line"></div>

            <language>
                <label for="language">Язык:</label>
                <select v-model="selectedLanguage" id="language">
                    <option value="ru">Русский</option>
                    <option value="en">Английский</option>
                </select>
            </language>

            <div class="line"></div>

            <button @click="updateUserData">Редактировать</button>
        </div>
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
            user: {}, // Данные о пользователе
            selectedLanguage: 'ru', // Значение по умолчанию
            selectedType: 'no',
            newUsername: '', // Добавляемые данные о пользователе
            newFirstName: '',
            newLastName: '',
            newAbout: '',
            newStatus: 0
        };
    },
    mounted() {
        this.fetchUserData(); // Получаем данные о пользователе при создании компонента
    },
    methods: {

        // Получение данных о пользователе
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
                    console.error('Ошибка при получении данных пользователя:', error);
                });
        },
        // Открытие окна выбора файла
        openFileInput() {
            const fileInput = document.getElementById('avatarInput');
            fileInput.click();
        },
        // Обработка выбора файла
        handleFileChange(event) {
            const formData = new FormData();
            formData.append('file', event.target.files[0]); // Добавляем выбранное фото в FormData

            const accessToken = localStorage.getItem('accessToken');

            axios.post(`${this.backendURL}/me/photo`, formData, {
                headers: {
                    Authorization: `Bearer ${accessToken}`,
                    'Content-Type': 'multipart/form-data'
                }
            })
                .then(response => {
                })
                .catch(error => {
                    console.error('Ошибка при загрузке фото:', error);
                });
        },
        // Обновление данных пользователя
        updateUserData() {
            const accessToken = localStorage.getItem('accessToken');

            axios.patch(`${this.backendURL}/me`, {
                username: this.newUsername || this.user.username,
                first_name: this.newFirstName || this.user.first_name,
                last_name: this.newLastName || this.user.last_name,
                about: this.newAbout || this.user.about,
                status: this.newStatus || this.user.status
            }, {
                headers: {
                    Authorization: `Bearer ${accessToken}`,
                    'Content-Type': 'application/json'
                }
            })
                .then(response => {
                    this.$emit('changePage', 'userPage');
                })
                .catch(error => {
                    console.error('Ошибка при обновлении данных пользователя:', error);
                    // Обработка ошибки при обновлении данных
                });
        }
    }
}
</script>


<style scoped>
.edit {
    position: absolute;
    top: 242px;
    left: 740px;
}

.avatar {
    width: 130px;
    height: 130px;
    border-radius: 65px;
}

.options {
    width: 757px;

    margin-left: 397px;
}

.profileHeader {
    width: 646px;
    height: 193px;

    background-image: url("/src/assets/img/profileHeader.svg");
    border-radius: 20px;

    display: grid;
    grid-template-columns: 185px 461px;
    grid-template-rows: 105px 87px;

}

photo {
    width: 130px;
    height: 130px;
    border-radius: 60px;

    background-color: #F6F1F1;

    cursor: pointer;

    grid-column: 1;
    grid-row: 1 / 2;

    margin-top: 46px;
    margin-left: 31px;

}

bgEditor {
    display: flex;

    align-items: center;
    padding: 4px 19px;
    gap: 24px;

    width: 153px;
    height: 27px;

    background: rgba(217, 217, 217, 0.5);
    backdrop-filter: blur(8px);
    border-radius: 6px;

    cursor: pointer;
    color: #fff;

    grid-column: 2;
    grid-row: 1;

    margin-top: 16px;
    margin-left: 250px;
}

name {
    grid-column: 2;
    grid-row: 2;

    margin-left: 0;
}

.username {
    width: 440px;
    height: 41px;

    color: #444444;
    margin-left: 0;

    font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 15px;
  line-height: 18px;
  
  /* Диплом/Текс */
  color: #444444;
}

.nametag {
    width: 440px;
    height: 41px;

    color: #444444;
    margin-left: 0;

    font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 15px;
  line-height: 18px;
  
  /* Диплом/Текс */
  color: #444444;
}

.editor {
    margin-top: 16px;

    background-color: #fff;
    border-radius: 20px;

    width: 646px;
    height: 296px;
}

description {
    display: flex;
    margin-top: 25px;
    margin-left: 31px;

    /* Roboto/14 */
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 19px;

    color: #8C8C8C;
}

description input {
    margin-left: 86px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    color: #444444;
}

profile {
    display: flex;
    display: flex;
    margin-top: 25px;
    margin-left: 31px;

    /* Roboto/14 */
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 19px;

    color: #8C8C8C;
}

language {
    display: flex;
    display: flex;
    margin-top: 25px;
    margin-left: 31px;

    /* Roboto/14 */
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 19px;

    color: #8C8C8C;
}

#language {


    border: none;

    border-radius: 4px;

    margin-left: 21px;
    background-color: #fff;

    /* Inter/16 */
    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    outline: none;

    color: #444444;
}

#language:focus {
    border-color: none;
}

#closeProfile {


    border: none;

    border-radius: 4px;

    margin-left: 21px;
    background-color: #fff;

    /* Inter/16 */
    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    display: flex;
    align-items: center;

    outline: none;

    color: #444444;
}

#closeProfile:focus {
    border-color: none;
}

.line {

    width: 566px;
    height: 2px;
    margin-left: 31px;
    margin-top: 24px;

    /* Диплом/Акцентный2 */
    background: #E0E6EF;

}

button {
    width: 312px;
    height: 41px;
    margin-top: 24px;
    margin-left: 167px;

    border-radius: 14px;

    border: none;
    outline: none;

    color: #fff;
    background-color: #6133B4;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 17px;

    cursor: pointer;
}

button:active {
    background-color: #0056b3;
}

@media (max-width: 425px) {
    .options {
        width: 375px;
        background-color: #0056b3;
        

        margin-left: 0px;
        margin-top: 80px;
    }

    .profileHeader {
        width: 375px;
        height: 319px;

        margin-left: 0px;
        margin-top: 100px;

        background-image: url("/src/assets/img/profileHeaderMob.svg");


    }

    photo {
        margin-left: 123px;
        margin-top: 50px;

    }

    name {
        margin-left: -180px;
        margin-top: 100px;

        width: fit-content;
    }

    .username {
        width: 360px;
        height: 41px;

        color: #444444;
        margin-left: 0;
        background-color: #F6F1F1;
    }

    .nametag {
        width: 360px;
        height: 41px;

        color: #444444;
        margin-left: 0;

        background-color: #F6F1F1;
    }

    .linee {

        width: 375px;
        height: 2px;

        margin-top: 0;
        margin-left: -20px;
        background: #E0E6EF;

    }

    .editor {
        width: 375px;

        margin-top: -10px;
    }

    .line {
        width: 357px;
        margin-left: 9px;
    }

    button {
        margin-left: 31px;
    }

    description input {
        margin-left: 60px;
        width: fit-content;

        font-family: 'Inter';
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 19px;
        display: flex;
        align-items: center;

        color: #444444;
    }

    .edit {
        position: absolute;
        top: 245px;
        left: 130px;
    }

    bgEditor {
        display: none;
    }
}
</style>