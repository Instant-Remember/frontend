<template>
  <popup-container @click="$emit('close')">
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
/* Стили для всплывающего окна (scoped, чтобы избежать конфликтов стилей) */
.popup {
  position: fixed;

  margin-left: auto;
  margin-top: auto;

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
</style>
