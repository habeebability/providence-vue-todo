<template>
  <div id="app" class="container">
    <!-- <CompletedTodos></CompletedTodos> -->
    <h1>My Projects</h1>

    <div class="button-navs">
      <button class="all" @click="allTasks">View All</button>
      <button class="completed" @click="completedTasks">Completed</button>
      <button class="ongoing" @click="ongoingTasks">Ongoing</button>
      <button class="add-new" @click="addNewTodo">Add New</button>
    </div>

    <div class="add-new-todo" v-if="newTodo">
      <input class="new-one" type="text" v-model="newOne" required />
      <button class="btn-new-one" @click="saveTodo">save</button>
    </div>

    <div class="todo-div" v-for="(todo, index) in todos" :key="todo.id">
      <div class="todo">
        <div
          class="todo-label"
          v-if="!todo.editMode"
          @dblclick="editTodo(todo)"
        >
          <p>{{ todo.title }}</p>
          <p>{{ todo.date }}</p>
        </div>

        <input
          v-else
          class="todo-edit"
          type="text"
          v-model="todo.title"
          @blur="doneEditing(todo)"
        />
      </div>
      <div class="icons-group">
        <div>
          <span
            v-if="todo.editMode"
            @click="doneEditing(todo)"
            class="save-edit"
            >SAVE</span
          >
          <span class="edit-mode" v-else @click="editTodo(todo)">EDIT</span>
        </div>
        <span @click="removeTodo(index)" class="delete"> &times;</span>
      </div>
    </div>
  </div>
</template>

<script>
// import { defineComponent } from '@vue/composition-api'

export default {
  data() {
    return {
      newTodo: false,
      newOne: "",
      idForTodo: 3,
      showCompleted: false,
      showOngoing: false,
      date: new Date().toISOString().substr(0, 10),

      todos: [
        { id: 1, title: "todo 1", status: "completed", editMode: false },
        { id: 2, title: "todo 2", status: "ongoing", editMode: false },
        { id: 3, title: "todo 3", status: "completed", editMode: false },
      ],
    };
  },
  methods: {
    addNewTodo() {
      this.newTodo = !this.newTodo;
    },

    saveTodo() {
      if (this.newOne == "" || this.newOne.trim() == false) {
        return;
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newOne,
        status: "ongoing",
        date: new Date().toISOString().substr(0, 10),
      });

      this.newOne = "";
      this.newTodo = !this.newTodo;

      this.idForTodo++;
    },

    completedTasks() {
      let completedTasks = this.todos;
      completedTasks = completedTasks.filter((todo) => {
        return todo.status == "completed";
      });
      console.log(completedTasks);
    },

    // ongoingTasks() {
    //   let ongoing = this.todos;
    //   ongoing = this.todos.filter((todo) => {
    //     return todo.status == "ongoing";
    //   });
    // },

    editTodo(todo) {
      todo.editMode = true;
    },

    doneEditing(todo) {
      todo.editMode = false;
    },

    removeTodo(index) {
      this.todos.splice(index, 1);
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
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
.button-navs {
  margin-bottom: 2rem;
}
.button-navs button {
  margin: 1rem;
  padding: 0.5rem 1rem;
  cursor: pointer;
  border-radius: 0.5rem;
}
.todo-div {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 500px;
  margin: 1rem auto;
  background: #eee;
  padding: 0 1rem;
}

.delete {
  font-size: 2rem;
  color: crimson;
  cursor: pointer;
  margin-left: 2rem;
}

.add-new {
  background-color: #42b983;
  color: white;
}

.todo {
  display: flex;
  align-items: center;
}

.todo-label {
  padding: 0 10px;
  border: 1px solid white;
  margin-left: 12px;
  font-size: 1rem;
}

.todo-edit {
  font-size: 1rem;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
}

.todo-edit:focus {
  outline: none;
}

.edit-mode {
  cursor: pointer;
}

.save-edit {
  cursor: pointer;
}
.icons-group {
  display: flex;
  align-items: center;
}
.new-one {
  padding: 0.5rem 2rem;
  margin-right: 2rem;
}

.btn-new-one {
  padding: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
</style>
