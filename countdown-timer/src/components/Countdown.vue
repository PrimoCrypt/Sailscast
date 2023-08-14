<script setup>
import { ref, computed } from 'vue'

const timeLeft = ref(0)
const inputTime = ref(18000)
const isTimerPaused = ref(null)
const isTimeWorking = ref(false)
const toggleTimerPause = ()=>{
    isTimerPaused.value = !isTimerPaused.value
}

const hours = computed(() => {
    const hours = Math.floor(timeLeft.value / 3600)
    return hours.toString().padStart(2, '0')
})
const minutes = computed(() => {
    const minutes = Math.floor((timeLeft.value % 3600)/60)
    return minutes.toString().padStart(2, '0')
})
const seconds = computed(() => {
    const seconds =  Math.floor(timeLeft.value % 60)
   return seconds.toString().padStart(2, '0')
})

const timer = ref(null)
const inputDisplay = ref(true)

const startCountdown =()=>{ 
    if( inputTime.value > 0){
        timeLeft.value = inputTime.value
        timer.value = setInterval(()=>{
            timeLeft.value--
            if(timeLeft.value === 0){
                clearInterval(timer)
            }
        }, 1000) 
        inputDisplay.value = false
    }
    isTimeWorking.value = true
}
const pauseCountdown = ()=>{
    if(timeLeft.value > 0){
        isTimerPaused.value = true
    }
    clearInterval(timer.value)
    isTimeWorking.value = true
}
const continueCountdown = ()=>{
    timer.value = setInterval(()=>{
            timeLeft.value--
            if(timeLeft.value === 0){
                clearInterval(timer)
            }
        }, 1000)
    toggleTimerPause()
}
const resetCountdown = ()=>{
    clearInterval(timer.value)
    timer.value = null
    timeLeft.value = 0
    inputDisplay.value = true
    isTimeWorking.value = false
    isTimerPaused.value = false
}


    
</script>

<template>
    <div>
        <h1 class="welcome">Countdown Timer</h1>
        <div class="timer">
            <h3 class="enter-time">
                Enter the time in seconds
            </h3>
            <input type="number" class="inputTime" v-model="inputTime" placeholder="200 seconds" v-if="inputDisplay">
            <div class="all-button">
                <button v-if="!isTimerPaused" @click="startCountdown" :disabled="isTimeWorking">Start</button>
                <button v-else @click="continueCountdown">Continue</button>
                <button @click="pauseCountdown" :disabled="!isTimeWorking">Pause</button>
                <button @click="resetCountdown"  :disabled="!isTimeWorking">reset</button>
            </div>
            <div class="full-time">
                <span class="time hours">{{ hours }}</span> :
                <span class="time minutes">{{ minutes }}</span> :
                <span class="time seconds">{{ seconds }}</span>
            </div>
        </div>
    </div>
</template>

<style scoped>
.welcome{
    font-size: 1.8rem;
}
.enter-time{
    margin: 35px 0 0;
    font-size: 1rem;
}
.inputTime{
    font-size: 1rem;
    background: #0e0066ca;
    margin: 0 0 20px 0;
}
.full-time{
    font-size: 2.5rem;
    margin: 1rem;
}

button{
    display: inline;
    margin: 0 5px;
}
</style>