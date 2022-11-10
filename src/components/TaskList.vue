<template>
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
</template>
<script>
import { EventBus } from "../utils/eventBus";
export default {
  props: ["task"],
  data() {
    return {
      editedItemIndex: null,
      tasks: []
    };
  },
  methods: {
    addTask(payload) {
      if (payload.subject.length === 0 || payload.type.length === 0) return;
      if (this.editedItemIndex !== null) {
        this.tasks.splice(this.editedItemIndex, 1, payload);
        // this.tasks[this.editedItemIndex].subject = this.task.subject;
        // this.tasks[this.editedItemIndex].type = this.task.type;
        this.editedItemIndex = null;
      } else {
        this.tasks.push(payload);
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      }
      //Bu inputu bosalda bilmedi
      //  this.task = {
      //   subject: "",
      //   type: "",
      //   status: false
      // };
      //1.Inputu yalniz bele bosalda bildim basqa yolu nedir?
      this.$emit('cleanInput');
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
      //2.Local storagede set olunmur edited item
    },
    changeStatus(taskItem) {
      this.task.status = taskItem.status;
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    }
  },
  created() {
    this.tasks = JSON.parse(localStorage.getItem("tasks")) || [];
  },
  mounted() {
    EventBus.$on("addTask", payload => {
      this.addTask(payload);
    });
  }
};
</script>
