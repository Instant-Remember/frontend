<template>
    <div class="progressBar">
      <input type="text" :value="progress" @input="onInput" class="percentage-input" /><span class="percentage-sign">%</span>
      <div class="progress-bar" ref="progressBar" @mousedown="startDrag">
        <div class="progress" :style="{ width: progress + '%' }"></div>
        <div class="progress-thumb" :style="{ left: progress + '%' }"></div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      progress: {
        type: Number,
        required: true
      }
    },
    data() {
      return {
        isDragging: false
      };
    },
    methods: {
      startDrag() {
        document.addEventListener('mousemove', this.onDrag);
        document.addEventListener('mouseup', this.stopDrag);
        this.isDragging = true;
      },
      onDrag(event) {
        if (this.isDragging) {
          const progressBar = this.$refs.progressBar;
          const rect = progressBar.getBoundingClientRect();
          const offsetX = event.clientX - rect.left;
          const newProgress = Math.min(Math.max(offsetX / rect.width * 100, 0), 100);
          this.updateProgress(newProgress);
        }
      },
      stopDrag() {
        document.removeEventListener('mousemove', this.onDrag);
        document.removeEventListener('mouseup', this.stopDrag);
        this.isDragging = false;
      },
      updateProgress(newProgress) {
        this.$emit('update:progress', newProgress);
      },
      onInput(event) {
        const newProgress = Math.min(Math.max(parseFloat(event.target.value), 0), 100);
        this.updateProgress(newProgress);
      }
    }
  };
  </script>

<style scoped>
.progressBar {
    display: flex;
    align-items: center;

    margin: 0px;
}

.progress-bar {
    margin-left: 8px;

    height: 7px;
    /* Высота шкалы */
    background-color: #E0E6EF;
    overflow: hidden;
    /* Добавлено для предотвращения выхода за границы */
    width: 591px;
    /* Ширина шкалы */

    border-radius: 20px;

    align-items: center;
    justify-content: center;

    width: calc(100% + 13px);

}

.progress {
    height: 100%;
    background-color: #6133B4;

    cursor: grab;

    border-radius: 20px;
}

.handle {
    top: 50%;
    transform: translateY(-70%);
    width: 13px;
    height: 13px;
    background-color: #6133B4;
    cursor: grab;
    border-radius: 20px;
    /* Радиус закругления для ползунка */
}

.percentage-input {
    width: 35px;

    text-align: right;

    background-color: #fff;

    color: #444444;

    width: 35px;
    height: 15px;

    font-family: 'Roboto';
    font-style: normal;
    font-weight: 200;
    font-size: 13px;
    line-height: 15px;
}

.percentage-sign {
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 200;
    font-size: 13px;
    line-height: 15px;
    color: #444444;
}
</style>