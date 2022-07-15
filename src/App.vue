<template>
  <div>
    <div class="todo">
      <div class="todo-head">
        <h1>MyTodo</h1>

        <div class="todo-add-task">
          <input
            type="checkbox"
            name=""
            id=""
            v-model="filterAllCompleted"
            v-if="showCheck"
          />
          <search-task
            v-if="searchandtask"
            v-model:searchTask="searchTask"
            @searching="searching"
            @searchandtask="search = false"
          ></search-task>

          <add-task v-else v-model:task="task" @addtask="addtask()" @searchandtask="search = true"></add-task>
        </div>
      </div>
      <div class="todo-body">
        <ul>
          <todo-task
            :task_item="task_item"
            v-for="task_item in getfilterTasks"
            :key="task_item.id"
            @editChange="editChange"
            @removeTask="removeTask"
            @doneEdit="doneEdit"
            @editCancel="editCancel"

          ></todo-task>
        </ul>
      </div>
      <div class="todo-footer" v-if="showCheck">
        <div class="left-item">{{ leftItemget }} item left</div>
        <div class="filter">
          <span @click="filter = 'all'" :class="{ current: filter === 'all' }">
            all
          </span>
          <span
            @click="filter = 'active'"
            :class="{ current: filter === 'active' }"
          >
            active
          </span>
          <span
            @click="filter = 'completed'"
            :class="{ current: filter === 'completed' }"
          >
            completed
          </span>
        </div>
        <div class="clear-completed" v-if="hasCompleted" @click="allclearCompleted()">
          clear-completed
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoTask from "./components/TodoTask";
import SearchTask from "./components/SearchTask";
import AddTask from "./components/AddTask";

export default {
  name: "App",
  components: {
    TodoTask,
    SearchTask,
    AddTask,
  },
  data() {
    return {
      searchTask: "",
      search: false,
      task: "",
      filter: "all",
      id: 3,
      taskCache: "",
      todo_lists_cache: [],
      todo_lists: [
        {
          id: 1,
          task: "task1",
          completed: true,
          edit: false,
        },
        {
          id: 2,
          task: "task2",
          completed: true,
          edit: false,
        },
      ],
    };
  },
  methods: {
    searching() {
      if (this.searchTask.trim() !== " ") {
        this.filter = "search";
      }
    },
    editCancel(task) {
      task.task = this.taskCache;
      task.edit = false;
    },
    doneEdit(task) {
      task.edit = false;
    },
    editChange(task) {
      this.todo_lists.filter((todo) => {
        todo.edit = false;
      });
      this.taskCache = task.task;
      task.edit = true;
    },
    addtask() {
      if (this.task.trim() == "") {
        return false;
      }
      let task = {
        id: this.id,
        task: this.task,
        completed: false,
        edit: false,
      };
      this.todo_lists.push(task);
      this.task = "";
      this.id++;
    },
    removeTask(id) {
      let remove_index = this.todo_lists.findIndex((task) => {
        return task.id === id;
      });

      this.todo_lists.splice(remove_index, 1);
    },
    allclearCompleted() {
      let clears = this.todo_lists.filter((task) => {
        return task.completed === false;
      });
      this.todo_lists = clears;
    },
  },
  computed: {
    searchandtask:{
      get(){
        return this.search;
      },
      set(data){
        console.log(data);
        return this.search = data;
      },
    },
    hasCompleted:{
      get(){
        return this.todo_lists.some((data)=>{
          return data.completed === true;
        })
      },
    },
    leftItemget: {
      get() {
        if (this.filter === "active") {
          return this.todo_lists.filter((task) => !task.completed).length;
        } else if (this.filter === "completed") {
          return this.todo_lists.filter((task) => task.completed).length;
        } else {
          return this.todo_lists.length;
        }
      },
    },
    filterAllCompleted: {
      set(check) {
        if (check) {
          this.todo_lists.filter((task) => {
            task.completed = true;
          });
        } else {
          this.todo_lists.filter((task) => {
            task.completed = false;
          });
        }
      },
      get() {
        return this.todo_lists.every((task) => {
          return task.completed === true;
        });
      },
    },
    showCheck() {
      return this.todo_lists.length === 0 ? false : true;
    },
    getfilterTasks: {
      get() {
        if (this.filter === "active") {
          return this.todo_lists.filter((task) => {
            return !task.completed;
          });
        } else if (this.filter === "completed") {
          return this.todo_lists.filter((task) => {
            return task.completed;
          });
        } else if (this.filter === "search") {
          return this.todo_lists.filter((task) => {
            return task.task.indexOf(this.searchTask) > -1;
          });
        } else {
          return this.todo_lists;
        }
      },
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

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.todo {
  width: 50%;
  margin: auto;
  box-shadow: 5px 5px #eee, 10px 10px #eee, 15px 15px #eee;
  border: 1px solid #eee;
  padding: 30px;
}

.todo-head {
  text-align: center;
  justify-content: center;
  align-items: center;
}

.todo-head input[type="text"] {
  width: 100%;
  height: 30px;
  font-size: 15px;
}

.todo-head input[type="text"]:focus {
  outline: none;
  box-shadow: -5px 5px 5px rgba(227, 230, 229, 0.456);
}

.todo-head input[type="checkbox"] {
  width: 10%;
  height: 30px;
}

.todo-add-task-leftside {
  display: flex;
  width: 100%;
}

.todo-add-task {
  display: flex;
  align-items: center;
}

.searchbtn {
  letter-spacing: 2px;
  border-radius: 0px;
  height: 35px;
  background-color: #eee;
  margin-left: 10px;
  cursor: pointer;
}

.todo-body ul {
  list-style: none;
}

.todo-body ul li {
  padding: 10px;
  border-bottom: 1px solid black;
}

.todo-task {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 20px;
  font-size: 18px;
  margin-bottom: 20px;
  margin-left: 10px;
}

.completedTodo {
  text-decoration: line-through;
}

.current {
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #eee;
  box-shadow: inset 5px 4px 5px #eee;
}

.todo-task:hover .todo-task-close {
  opacity: 1;
}

.todo-task-close {
  cursor: pointer;
  opacity: 0;
  color: red;
  font-size: 15px;
}

.todo-task input[type="checkbox"] {
  width: 30px;
  height: 15px;
}

.todo-footer {
  display: flex;
  font-size: 17px;
  margin-left: 50px;
  margin-top: 30px;
  justify-content: space-between;
}

.todo-footer .filter span {
  margin: 5px;
  text-decoration: underline;
  cursor: pointer;
  color: rgb(86, 86, 211);
}

.todo-footer .clear-completed {
  text-decoration: underline;
  cursor: pointer;
  color: rgb(86, 86, 211);
}
</style>
