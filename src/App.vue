<template>
  <div id="app">
    <div id="brand">
      <Brand/>
    </div>
    <div id="container">
      <Clock/>
      <TasksDone id="task"/>
    </div>
    
  </div>
</template>

<script>
import Brand from './components/Brand';
import Clock from './components/Clock';
import TasksDone from './components/TasksDone'

export default {
  name: 'App',
  components: {
    Brand,
    Clock,
    TasksDone
  },
  
}
</script>

<style>
body{
  background-color: #D84747;
}

#brand{
  margin: 1.5rem;
  position: relative;
  display: flex;
  justify-content: center;
}

#container{
  margin: 1rem;
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

#task{
  margin-left: 10rem;
}

</style>
