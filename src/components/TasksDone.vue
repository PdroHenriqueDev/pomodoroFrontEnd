<template>
    <div class="task-container">
        <p class="tasksdone">Completed:</p>
        <div class="tasks-info" v-for="(task, index) in tasks" :key="index">
            <p><span>Task: </span> {{task.taskName}}</p>
            <p><span>Time: </span>{{task.taskTime}} minutes</p>
            <IconifyIcon :icon="icons.circleX" :style="{color: '#fff', fontSize: '25px'}" @click="remove(task.id)" class="icon-x"/>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import IconifyIcon from '@iconify/vue';
import circleX from '@iconify/icons-akar-icons/circle-x';

export default {
    components: {
        IconifyIcon
    },
    data() {
        return{
          tasks: [],
          icons: {
			circleX,
		},  
        }
    },
    methods: {
        async remove(id){
            await axios.delete("http://localhost:2525/" + id)
            .then(response => {console.log(response)})
            .catch(err => {console.log(err)
             alert(err)})
             
             window.location.reload()
        }
    },
    created: async function(){
        await axios.get("http://localhost:2525/tasks")
         .then(response => {
                this.tasks = response.data
                console.log(response.data)
            }).catch(err => {
                console.log(err)
                alert(err)
            })
    },
    
}
</script>


<style scoped>
.task-container{
    width: 400px;
    max-width: 500px;
    max-height: 70vh;
    background-color: #BD4040;
    overflow: auto;
    overflow-x: auto;
}

.tasksdone{
    text-align: center;
    color: #fff;
    font-family: Roboto;
    font-size: 30px;
    font-weight: 400;
    line-height: 35px;
}

.tasks-info{
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;
}

.tasks-info p {
    color: #fff;
    font-family: Roboto;
    font-size: 18px;
    font-weight: 400;
    line-height: 26px;
}

.icon-x {
    cursor: pointer;
}

</style>