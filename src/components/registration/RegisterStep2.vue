<template>
    <div>

        <input v-model="username" placeholder="Имя пользователя" class="username">
        <input v-model="email" placeholder="Почта" class="email">

        <div class="steps">
            <div class="step1"></div>
            <div class="step2"></div>
            <div class="step3"></div>
        </div>

        <button @click="nextStep">Дальше</button>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            username: '',
            email: '',
            activeStep: 1,
        };
    },
    methods: {
        nextStep() {
            // Валидация и переход на следующий шаг
            if (this.username && this.email) {
                this.activeStep = 3;
                this.$emit('nextStep', { username: this.username, email: this.email });
            } else {
                alert('Введите имя пользователя и почту');
            }
        },
    },
};
</script>
  
<style scoped>
.username {
    margin-top: 24px;
}

.email {
    margin-top: 24px;
}

input {
    background: #E8ECF2;
    border-radius: 20px;

    width: 312px;
    height: 50px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 17px;

    display: flex;
    align-items: center;

    margin-left: 56px;

}

input::placeholder {
    text-indent: 20px;
}

.steps {
    width: 312px;
    height: 8px;

    margin-top: 24px;
    margin-left: 62px;

    display: flex;
}

.step1 {
    width: 95px;
    height: 8px;

    background: #6133B4;
    border-radius: 4px;
}

.step2 {

    width: 95px;
    height: 8px;

    background: #6133B4;
    border-radius: 4px;

    margin-left: 14px;
}

.step3 {

    width: 95px;
    height: 8px;

    background: #E0E6EF;
    border-radius: 4px;

    margin-left: 14px;
}

button {
    width: 312px;
    height: 49px;

    margin-left: 56px;
    margin-top: 24px;

    border-radius: 20px;

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
</style>
  