<template>
  <b-container class="todo">
    <b-row>
      <b-col md="8" offset-md="2">
        <h3 class="mb-4">Todo App</h3>
      </b-col>
      <b-col md="8" offset-md="2" class="input-todo">
        <b-input-group>
          <b-form-input
            type="text"
            placeholder="add new todo"
            v-model="newTodo"
            @keyup.enter="addTodo"
          ></b-form-input>

          <b-input-group-append>
            <b-button variant="outline-dark" @click="addTodo">add</b-button>
          </b-input-group-append>
        </b-input-group>
      </b-col>

      <b-col md="8" offset-md="2" class="mt-2 list-todo">
        <p v-if="!todos.length" class="pt-3">
          Belum ada todo nih ayo tambahkan !
        </p>

        <b-list-group>
          <b-list-group-item
            v-for="todo in todos"
            :key="todo.id"
            class="mb-1 d-flex align-items-center"
            :class="todo.done ? 'done' : ''"
          >
            <p class="mb-0">{{ todo.todo }}</p>
            <div class="list-actions ml-auto d-flex">
              <b-btn
                variant="dark"
                size="sm"
                class="mr-1 p-0"
                :data-id="todo.id"
                @click="deleteTodo"
                title="delete"
              >
                <span class="material-icons">
                  close
                </span>
              </b-btn>

              <b-btn
                size="sm"
                class="p-0"
                @click="todo.done = !todo.done"
                :title="todo.done ? 'undone' : 'done'"
                :variant="todo.done ? 'primary' : 'success'"
              >
                <span class="material-icons">
                  {{ todo.done ? "sync" : "check" }}
                </span>
              </b-btn>
            </div>
          </b-list-group-item>
        </b-list-group>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length) {
        this.todos.unshift({
          id: this.todos.length + 1,
          todo: this.newTodo,
          done: false,
        });
      }
      this.newTodo = "";
    },
    deleteTodo(e) {
      this.todos = this.todos.filter((todo) => todo.id != e.target.dataset.id);
    },
  },
  watch: {
    todos(newTodos) {
      const todos = JSON.stringify(newTodos);
      localStorage.setItem("todos", todos);
    },
  },
  mounted() {
    if (localStorage.getItem("todos")) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }
  },
};
</script>

<style>
.done {
  text-decoration: line-through;
}

p {
  font-size: 14px;
}
.material-icons {
  padding: 4px;
  pointer-events: none;
}

.list-group-item {
  background-color: #fff !important;
}
</style>
