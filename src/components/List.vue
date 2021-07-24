<template>
  <div class="d-flex justify-content-center">
    <div class="p-2">Task Total : {{ todos.length }}</div>
  </div>
  <div class="list-group">
    <div 
      class="list-group-item"
      :class="{ done: todo.is_done }"
      v-for="(todo, todoIdx) in todos" :key="todoIdx"
    >
      <div class="row">
        <div class="col" :class="{'text-decoration-line-through':todo.is_done}">
          {{ todo.activity }}
        </div>
        <div class="col-auto">
          <div class="row gx-1">
            <div class="col">
              <button 
                class="btn btn-sm btn-primary"
                @click="swapIsDone(todoIdx)"
              >
                {{ todo.is_done ? 'undone' : 'done' }}
              </button>
            </div>
            <div class="col">
              <button 
                class="btn btn-sm btn-warning text-white"
                @click="$emit('todo:edit', todoIdx)"
              >
                edit
              </button>
            </div>
            <div class="col">
              <button 
                class="btn btn-sm btn-danger"
                @click="$emit('todo:delete', todoIdx)"
              >
                X
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  emits: ["is_done:swap", "todo:delete", "todo:edit"],
  setup(props, { emit }) {
    // ---- methods ----
    const swapIsDone = (todoIdx) => emit('is_done:swap', todoIdx);
    // ---- end methods ----

    return {
      swapIsDone
    }
  },
};
</script>

<style scoped>
.done {
  background-color: #fff5ea
}

.list-group-item {
  animation: slide-up 1s ease;
}

@keyframes slide-up {
  0% {
      opacity: 0;
      transform: translateY(-30px);
  }
  100% {
      opacity: 1;
      transform: translateY(0);
  }
}
</style>