<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">SIMPLE TO DO APP</h5>
      <div class="input-group">
        <input 
          type="text" 
          class="form-control" 
          placeholder="input task"
          v-model="todo"
          @keyup.enter="addTask"
        />
        <button 
          type="button" 
          class="btn btn-outline-secondary"
          @click="addTask"
        >
          + Insert
        </button>
      </div>

      <list 
        v-if="values.length > 0" 
        :todos="values"  
        @is_done:swap="swapIsDone"
        @todo:delete="deleteTask"
      />
    </div>
  </div>
</template>

<script>
import { ref, reactive, toRefs, onMounted } from "vue";
import component from "./components"

export default {
  components: { 
    List: component.List
  },
  setup(props) {
    const todo = ref("");
    const todos = reactive({
      values: []
    });

    // ---- method ----
    const addTask = () => {
      todos.values.unshift({
        activity: todo.value,
        is_done: false
      });

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

    const saveToLocalStorage = () => localStorage.setItem("todo-list", JSON.stringify(todos.values));
    // ---- end method ----

    // ---- lifecycle ----
    onMounted(() => {
      todos.values = JSON.parse(localStorage.getItem("todo-list")) ?? [];
    });
    // ---- end lifecycle ----

    return {
      todo,
      ...toRefs(todos),
      addTask,
      swapIsDone,
      deleteTask
    }
  },
};
</script>