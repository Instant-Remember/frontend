<template>
    <div class="options">
        <div class="profileHeader">
            <photo>
                <img src="/src/assets/img/avatarEdit.svg" alt="">
            </photo>

            <bgEditor>
                <img src="/src/assets/img/pencil.svg" alt="">
                <p>Изменить</p>
            </bgEditor>


            <name>
                <input type="text" placeholder="Иван Глобиков" class="username">
                <input type="text" placeholder="@ivang" class="nametag">
            </name>
        </div>

        <div class="editor">
            <description>
                <p>Описание:</p>
                <input type="text" placeholder="миллионер">
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
            
            <button>Редактировать</button>
        </div>

    </div>
</template>

<script>
export default {
    data() {
        return {
            selectedLanguage: 'ru', // значение по умолчанию
            selectedType: 'no'
        };
    },
};
</script>

<style scoped>
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
    width: 457px;
    height: 41px;

    color: #444444;
    margin-left: 0;
}

.nametag {
    width: 457px;
    height: 41px;

    color: #444444;
    margin-left: 0;
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
}</style>