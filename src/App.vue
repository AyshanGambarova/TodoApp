<template>
  <div id="app">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-6">
          <AddTask :task="task" />
          <TaskList :task="task" @cleanInput='cleanInput'/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AddTask from "./components/AddTask.vue";
import TaskList from "./components/TaskList.vue";
export default {
  name: "app",
  components: { AddTask, TaskList },
  data() {
    return {
      task: {
        subject: "",
        type: "",
        status: false
      }
    };
  },
  methods: {
    cleanInput() {
      this.task = {
        subject: "",
        type: "",
        status: false
      };
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
