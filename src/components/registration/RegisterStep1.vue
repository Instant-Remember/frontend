<template>
  <div>
    <input v-model="userData.first_name" placeholder="Имя" class="name">
    <input v-model="userData.last_name" placeholder="Фамилия" class="surname">
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
          userData: {
              first_name: '',
              last_name: ''
          }
      };
  },
  methods: {
      nextStep() {
          // Проверка на заполненность полей и переход на следующий шаг
          if (this.userData.first_name && this.userData.last_name) {
              this.$emit('nextStep', this.userData);
          } else {
              alert('Введите имя и фамилию');
          }
      },
  },
};
</script>

<style scoped>
.name {
  margin-top: 24px;
}

.surname {
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

  background: #E0E6EF;
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
