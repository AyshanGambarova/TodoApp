<template>
  <div>
    <div class="d-flex">
      <div class="me-2 w-100">
        <input
          type="text"
          class="form-control"
          placeholder="Subject..."
          v-model="task.subject"
        />
      </div>
      <div class="me-2">
        <select v-model="task.type" class="form-select">
          <option
            v-for="(item, index) in taskTypes"
            :key="index"
            :value="item"
            >{{ item }}</option
          >
        </select>
      </div>
      <div>
        <button @click="submitTask" class="btn btn-primary">Add</button>
      </div>
    </div>
    <div class="mt-3" v-if="tasks != null && tasks.length">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Subject</th>
            <th scope="col">Type</th>
            <th scope="col">Status</th>
            <th scope="col"></th>
            <th scope="col"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td :class="[task.status ? 'complated' : '']">
              {{ task.subject }}
            </td>
            <td>{{ task.type }}</td>
            <td>
              <input
                type="checkbox"
                v-model="task.status"
                @change="changeStatus(task)"
              />
            </td>
            <td>
              <button
                class="btn btn-sm btn-success"
                @click="editTask(index, task)"
              >
                Edit
              </button>
            </td>
            <td>
              <button class="btn btn-sm btn-danger" @click="deleteTask(index)">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div v-else class="mt-5">You don't have any task.</div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      taskTypes: ["Frontend", "Database", "Backend", "UX/UI"],
      task: {
        subject: "",
        type: "",
        status: false
      },
      tasks: [],
      editedItemIndex: null
    };
  },
  created() {
    this.tasks = JSON.parse(localStorage.getItem("tasks")) || [];
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedItemIndex !== null) {
        this.tasks.splice(this.editedItemIndex, 1, this.task);
        // this.tasks[this.editedItemIndex].subject = this.task.subject;
        // this.tasks[this.editedItemIndex].type = this.task.type;
        this.editedItemIndex = null;
      } else {
        this.tasks.push(this.task);
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      }
      console.log(this.tasks);
      this.task = {
        subject: "",
        type: "",
        status: false
      };
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    editTask(index, editedTask) {
      this.task.subject = editedTask.subject;
      this.task.type = editedTask.type;
      this.editedItemIndex = index;
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    changeStatus(taskItem) {
      this.task.status = taskItem.status;
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    }
  }
  // watch: {
  //   task: {
  //     handler(newValue) {
  //       console.log(newValue);
  //     },
  //     deep: true
  //   }
  // }
};
</script>
<style>
.complated {
  text-decoration: line-through;
}
</style>
