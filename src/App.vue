<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title mb-4 text-center">TASKLIST APP</h5>
      <div class="input-group">
        <input 
          type="text" 
          class="form-control shadow-none" 
          placeholder="Input New Task"
          v-model="todo"
          @keyup.enter="saveTask"
        />
        <button 
          type="button" 
          class="btn btn-outline-secondary"
          @click="saveTask"
        >
          + {{ updateId === "" ? "insert" : "update" }}
        </button>
      </div>

      <list 
        v-if="values.length > 0" 
        :todos="values"  
        @is_done:swap="swapIsDone"
        @todo:delete="deleteTask"
        @todo:edit="updateTask"
      />

    </div>
    <footer>&copy; Copyright {{ year }}, Created by @ferdian</footer>
  </div>
</template>

<script>
import { ref, reactive, toRefs, onMounted } from "vue";
import component from "./components"

export default {
  components: { 
    List: component.List
  },
  setup() {
    const todo = ref("");
    const todos = reactive({
      values: []
    });
    const updateId = ref("");
    const year = new Date().getFullYear();

    // ---- method ----
    const saveTask = () => {
      if (updateId.value !== "") {
        todos.values = todos.values.filter((item, index) => {
          if (index === updateId.value) item.activity = todo.value;
          return item;
        });
      } else {
        todos.values.unshift({
          activity: todo.value,
          is_done: false
        });
      }

      todo.value = "";

      saveToLocalStorage();
    }

    const swapIsDone = (todoIdx) => {
      todos.values = todos.values.filter((item, index) => {
        if (index === todoIdx) item.is_done = !item.is_done;
        return item;
      })
      saveToLocalStorage();
    }

    const deleteTask = (todoIdx) => {
      todos.values = todos.values.filter((item, index) => {
        if (index !== todoIdx) return item;
      });
      saveToLocalStorage();
    }

    const updateTask = (todoIdx) => {
      todo.value = todos.values[todoIdx].activity;
      updateId.value = todoIdx;
    }

    const saveToLocalStorage = () => {
      localStorage.setItem("todo-list", JSON.stringify(todos.values));
      updateId.value = "";
    }
    // ---- end method ----

    // ---- lifecycle ----
    onMounted(() => {
      todos.values = JSON.parse(localStorage.getItem("todo-list")) ?? [];
    });
    // ---- end lifecycle ----

    return {
      todo,
      ...toRefs(todos),
      saveTask,
      swapIsDone,
      deleteTask,
      updateTask,
      updateId,
      year
    }
  },
};
</script>

<style>
  body {
    background-color: #dff0ff;
  }

  footer {
    margin-top: 20px;
    background-color: rgb(240, 240, 240);
    color: rgb(97, 97, 97);
    text-align: center;
    padding: 5px;
    font-size: .7rem;
    font-family:Arial, Helvetica, sans-serif;
  }
</style>