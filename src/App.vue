<template>
  <div id="app">
    <b-btn class="toggleSidebar" size="sm" v-b-toggle.sidebar>
      <span class="material-icons">
        menu
      </span>
    </b-btn>
    <pomodoro-component></pomodoro-component>

    <!-- sidebar -->
    <b-sidebar id="sidebar" lazy width="100%">
      <div class="pt-5">
        <todo-component></todo-component>
      </div>
    </b-sidebar>
  </div>
</template>

<script>
import Todo from "./components/todo";
import Pomodoro from "./components/pomodoro";
export default {
  name: "App",
  components: {
    "todo-component": Todo,
    "pomodoro-component": Pomodoro,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  position: relative;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: whitesmoke;
  color: #343a40;
}

#app .toggleSidebar {
  position: absolute;
  right: 8px;
  top: 8px;
  padding: 2px 4px;
  background-color: whitesmoke;
  border: whitesmoke;
  color: #343a40;
}

#sidebar {
  background-color: whitesmoke !important;
}
</style>
