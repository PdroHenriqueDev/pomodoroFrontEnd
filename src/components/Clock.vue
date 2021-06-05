<template>
    <div class="clock-container">
        <div class="timers-buttons">
            <button @click="changeTimer(0), saveIsOk()">{{timers[0].name}}</button>
            <button @click="changeTimer(1), cantSave()">{{timers[1].name}}</button>
            <button @click="changeTimer(2), cantSave()">{{timers[2].name}}</button>
        </div>

        <div class="timer">
            <h1>{{displayMinutes}}:{{displaySeconds}}</h1>
        </div>

        <div class="action-buttons">
            <button @click="start">Start</button>
            <button @click="stop">Stop</button>
            <button @click="reset(currentTimer)">Reset</button>
        </div>

        <div class="form-container" v-show="finished">
            
                <label >Time: 25 minutes</label>
                <label>Name:</label>
                <input id="name" type="text" name="taskName" placeholder="Task">
                <button @click="send()" type="submit">Save</button>
            
        </div>

    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            timers: [
                {
                    name: "Pomodoro",
                    minutes: 25
                },
                {
                    name: "Short Break",
                    minutes: 5
                },
                {
                    name: "Long Break",
                    minutes: 15
                }
            ],
            totalSeconds: 25 * 60,
            isRunning: false, 
            timerInstance: null,
            currentTimer: 0,
            canSave: true,
            save: false,
            finished: false,
            taskName: " ",
        }
    },
    computed: {
         displayMinutes(){
            const minutes = Math.floor(this.totalSeconds / 60) 
            return this.formatTime(minutes)
         },
         displaySeconds(){
            const seconds = this.totalSeconds % 60
            return this.formatTime(seconds)
        }
    },
    methods: {
        formatTime(time){
            if(time < 10) {
                return '0' + time
            }
            return time.toString()
        },
        changeTimer(i){
            this.stop()
            this.finished = false
            this.totalSeconds = this.timers[i].minutes * 60
            return this.currentTimer = i
        },
        start(){
            this.stop()
            this.isRunning = true
            this.timerInstance = setInterval(() => {
                if(this.totalSeconds <= 0){
                    this.stop()
                    this.save = true
                    if(this.save && this.canSave){
                        this.finished = true
                    }
                    return 
                }
                this.totalSeconds -= 1 
            }, 1000)
        },
        stop(){
            this.isRunning = false
            clearInterval(this.timerInstance)
        },
        reset(i){
            this.stop()
            this.currentTimer = i
            this.totalSeconds = this.timers[i].minutes * 60
        },
        cantSave(){
          this.canSave = false  
        },
        saveIsOk(){
            this.canSave = true
        },
        send:  async function(){
            const name = document.getElementById("name")
            const value = name.value
            const task = {taskTime: 25, taskName: value}
      
         await axios.post("http://localhost:2525/save", task)
         .then(response => {
                console.log(response)
            }).catch(err => {
                console.log(err)
                alert(err)
            })
            window.location.reload()
        },
        
    }
    
}
</script>

<style scoped>
.clock-container{
    display: flex;
    flex-direction: column;
    justify-items: center;
    align-items: center;
    background-color: #BD4040;
    width: 600px;
    height: 65vh;
}

.timers-buttons{
    margin: 1rem;
    width: 100%;
    display: flex;
    justify-content: space-between;
}

.timers-buttons button:first-child {
    margin-left: 0.5rem;
}

.timers-buttons button:last-child {
    margin-right: 0.5rem;
}

.timers-buttons button{
    font-family: Russo One;
    font-size: 30px;
    font-style: normal;
    font-weight: 800;
    line-height: 48px;
    color: #FFF;
    background: none;
    border: none;
    border-radius: 0.25rem;
    cursor: pointer; 
}

.timers-buttons button:hover {
    background-color: #993636;
    outline: none;
}

.timer {
    font-family: Russo One;
    font-size: 35px;
    font-style: normal;
    font-weight: 600;
    line-height: 121px;
    color: #FFF;
}

.action-buttons {
    display: flex;
    justify-content: space-between;
    width: 70%;
}

.action-buttons button {
    font-family: Russo One;
    font-size: 40px;
    font-style: normal;
    font-weight: 400;
    line-height: 48px;
    text-align: left;
    color: #FFF;
    border-radius: 4px;
    background-color: #993636;
    cursor: pointer;
}

.form-container {
    margin-top: 1.5rem;
    width: 80%;
    display: flex;
    justify-content: space-evenly;
}

.form-container label{
    font-family: Russo One;
    font-size: 20px;
    font-weight: 400;
    line-height: 24px;
    color: #FFF;
}

.form-container input {
    border: 3px solid #D84747;
    border-radius: 5px;
}

.form-container button {
    background-color: #FFF;
    border: none;
    width: 3rem;
    border-radius: 5px;
    cursor: pointer;
}

</style>