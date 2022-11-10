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
  </div>
</template>
<script>
import { EventBus } from "../utils/eventBus";
export default {
  props: ["task"],
  data() {
    return {
      taskTypes: ["Frontend", "Database", "Backend", "UX/UI"]
    };
  },
  methods: {
    submitTask() {
      EventBus.$emit("addTask", this.task);
    }
  }
};
</script>
<style>
.complated {
  text-decoration: line-through;
}
</style>
