<template lang="">
  <div>
    <li class="todo-task">
      <div>
        <input type="checkbox" name="" id="" v-model="task.completed" />
        <input
          type="text"
          v-focus
          v-if="task.edit"
          class="edittask"
          v-model="task.task"
          v-on:blur="doneEdit(task)"
          @keyup.esc="editCancel(task)"
        />
        <span
          :class="{ completedTodo: task.completed }"
          @dblclick="editChange(task)"
          v-else
          >{{ task.task }}</span
        >
      </div>

      <span class="todo-task-close" @click="removeTask(task.id)">X</span>
    </li>
  </div>
</template>
<script>
export default {
  name: "TodoTask",
  props: ["task_item", "a"],
  data() {
    return {
      aq: this.a,
      task: this.task_item,
    };
  },
  methods: {
    doneEdit(task) {
      this.$emit("doneEdit", task);
    },
    editChange(task) {
      this.$emit("editChange", task);
    },
    removeTask(id) {
      this.$emit("removeTask", id);
    },
    editCancel(task) {
      this.$emit("editCancel", task);
    },
  },
  directives: {
    focus: {
      mounted: function (el) {
        el.focus();
      },
    },
  },
};
</script>
<style lang=""></style>
