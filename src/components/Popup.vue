<template>
  <popup-container @click="$emit('close')">
    <button @click="$emit('close')">Х</button>
    <div class="popup" @click.stop>
      <slot>
      </slot>
    </div>

  </popup-container>
</template>

<script>
export default {
  name: 'Popup',
};
</script>

<style scoped>
button {
  background: rgba(44, 44, 44, 0.5);
}

/* Стили для всплывающего окна (scoped, чтобы избежать конфликтов стилей) */
.popup {
  position: fixed;

  margin-left: auto;
  margin-top: 10px;

  background-color: #fff;
  backdrop-filter: blur(12px);

  border-radius: 20px;

  width: 687px;

  padding-bottom: 16px;
}

popup-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(44, 44, 44, 0.5);
  backdrop-filter: blur(6px);

  display: flex;
  justify-content: center;
  align-items: center;
}

@media  (max-width: 425px){
  .popup {
    position: fixed;
    left: 3%;
    top: 56px;

    background-color: #fff;
    backdrop-filter: blur(12px);

    border-radius: 20px;

    width: 375px;
    height: 419px;

    padding-bottom: 16px;
  }

  popup-container {
    width: 100vh;
    top: 58px;
    border-radius: 20px;
  }

  button {
    width: 24px;
    height: 24px;

    position: fixed;
    top: 16px;
    left: 335px;
    border-radius: 20px;
    background: none;
  }
}

@media (max-width: 375px) {
  .popup {
    position: fixed;
    left: 0%;
    top: 56px;

    background-color: #fff;
    backdrop-filter: blur(12px);

    border-radius: 20px;

    width: 375px;
    height: 419px;

    padding-bottom: 16px;
  }

  popup-container {
    width: 100vh;
    top: 58px;
    border-radius: 20px;
  }

  button {
    width: 24px;
    height: 24px;

    position: fixed;
    top: 16px;
    left: 335px;
    border-radius: 20px;
    background: none;
  }
}
</style>
