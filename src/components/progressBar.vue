<template>
    <div class="progressBar">
        <input type="text" v-model="progress" @input="updateProgress" class="percentage-input" /><span class="percentage-sign">%</span>

        <div class="progress-bar" @mousedown="startDrag" ref="progressBar">
            <div class="progress" :style="{ width: progressBarWidth }"></div>
            <!--<div class="handle" :style="{ left: handlePosition }" ref="handle"></div>--> 
        </div>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            progress: 0,
            isDragging: false
        };
    },
    computed: {
        progressBarWidth() {
            return this.progress + '%';
        },
        handlePosition() {
            return `calc(${this.progress}% - 6.5px)`; // Рассчитываем положение ползунка с учетом его размера
        }
    },
    methods: {
        startDrag() {
            this.isDragging = true;
            document.addEventListener('mousemove', this.handleDrag);
            document.addEventListener('mouseup', this.stopDrag);
        },
        handleDrag(event) {
            if (this.isDragging) {
                const progressBar = this.$refs.progressBar;
                const handle = this.$refs.handle;
                const rect = progressBar.getBoundingClientRect();
                const offsetX = event.clientX - rect.left;
                const percentage = Math.min(100, Math.max(0, (offsetX / rect.width) * 100));
                this.progress = Math.round(percentage); // Округляем значение
            }
        },
        stopDrag() {
            this.isDragging = false;
            document.removeEventListener('mousemove', this.handleDrag);
            document.removeEventListener('mouseup', this.stopDrag);
        },
        updateProgress() {
            // Дополнительная проверка, чтобы избежать NaN
            if (isNaN(this.progress) || this.progress < 0) {
                this.progress = 0;
            } else if (this.progress > 100) {
                this.progress = 100;
            }
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
    justify-content:center;

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
  