<script setup>
import {DateTime} from 'luxon';
import {ref, reactive, computed, onBeforeUnmount} from 'vue'

const currentTime = ref(DateTime.now());
const started = ref();
const mode = ref('');
const limitMinutes = ref(0);
const updateCurrentTime = () => {
    currentTime.value = DateTime.now();
};

const updateTimeInterval = setInterval(updateCurrentTime, 1000);
onBeforeUnmount(() => {
    clearInterval(updateTimeInterval);
});

function pad(number, width) {
    number = parseInt(number);
    number = String(number);
    return number.padStart(width, '0');
}

const time = computed(() => {
    if (!mode.value) {
        return
    }

    let diff = currentTime.value.diff(started.value, ['seconds', 'hours', 'minutes']);
    let minutes = diff.minutes;
    let seconds = diff.seconds;

    if (mode.value === 'down') {
        minutes = limitMinutes.value - minutes - 1;
        seconds = (60 - seconds) % 60;
    }
    return `${pad(minutes, 2)}:${pad(seconds, 2)}`

    // return currentTime.value.toFormat('HH:mm:ss')
})

function start() {
    mode.value = 'up';
    limitMinutes.value = 0;
    started.value = DateTime.now();
}

function reverse() {
    mode.value = 'down';
    limitMinutes.value = 10;
    started.value = DateTime.now()
}

</script>

<template>
    <div class="clock">
        <div class="background">
            88:88
        </div>
        <div class="foreground">
            {{ time }}
        </div>

    </div>

    <button @click="start">start</button>
    <button @click="reverse">reverse</button>
</template>

<style src="dseg/css/dseg.css">
</style>

<style>


body {
    min-height: 100vh;
    color: #fff;
    background: #000;
    line-height: 1.6;
    font-family: Inter,
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    Oxygen,
    Ubuntu,
    Cantarell,
    'Fira Sans',
    'Droid Sans',
    'Helvetica Neue',
    sans-serif;
    font-size: 15px;
    text-rendering: optimizeLegibility;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

.clock {
    font-family: DSEG7-Classic;
    font-size: 150px;
    font-weight: 800;
    position: relative;

    .background {
        top: 0;
        font-weight: 400;
        left: 0;
        user-select: none;
        color: #111;
    }

    .foreground {
        position: absolute;
        font-weight: 400;
        top: 0;
        left: 0;
        color: #ff3e63;
    }
}


</style>
