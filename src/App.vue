<template>
  <div>
    <md-card>
      <md-field>
        <md-input
          v-model="currentTodo"
          @keydown.enter="addTodo()"
          placeholder="Add a todo..."
        ></md-input>
      </md-field>
      <ul class="todos">
        <li class="todo-item" v-for="todo in todos" :key="todo.id">
          <input
            class="completed"
            type="checkbox"
            @change="togglecompleted"
            v-model="todo.completed"
          />
          <span
            v-if="todo.editing === false"
            class="editing"
            :class="{ editing: todo == editedTodo }"
            @dblclick="editTodo(todo)"
          >
            {{ todo.label }}
          </span>
          <md-field v-if="todo.editing" class="editing">
            <md-input
              type="text"
              v-model="todo.label"
              @keyup.enter="stopEdit(todo)"
              @keyup.esc="stopEdit(todo)"
              @focusout="stopEdit(todo)"
              placeholder="Add item here"
            />
          </md-field>
          <button v-if="todo.editing === false" @click="editTodo(todo)">
            Edit
          </button>
          <button v-if="todo.editing === true" @click="stopEdit(todo)">
            Save
          </button>
          <button v-if="todo.editing === false" @click="removeTodo(todo)">
            Delete
          </button>
          <button v-if="todo.editing === true" @click="editTodo(todo)">
            Cancel
          </button>
        </li>
      </ul>
    </md-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      currentTodo: '',
      editedTodo: '',
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.todos.length,
        label: this.currentTodo,
        completed: false,
        editing: false,
      });
      this.currentTodo = '';
    },
    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      const index = this.todos.indexOf(todo);
      this.todos[index].editing = !this.todos[index].editing;
    },
    stopEdit(todo) {
      todo.editing = false;
    },
  },
};
</script>

<style>
body {
  font-family: 'Roboto', sans-serif;
  font-size: 18px !important;
  line-height: 1.5 !important;
  margin-left: 30px !important;
  margin-right: 30px !important;
  background-color: #2b3e50;
}

.md-card {
  width: 50% !important;
  margin-left: auto;
  margin-right: auto;
  margin-top: 20px;
  background-color: #df691a !important;
  color: #ffffff;
  text-transform: capitalize;
  border-radius: 3px;
}

ul {
  list-style-type: none;
  margin-left: 0px;
  padding: 0;
}

button {
  margin: 3px;
}

span {
  padding-left: 3px !important;
  padding-right: 3px !important;
}

.md-field {
  border: 2px solid white !important;
  height: 40px !important;
  border-radius: 3px !important;
  width: 60% !important;
  font-size: 20px !important;
  min-height: 0px !important;
  font-size: 20px !important;
  padding-top: 5px !important;
  padding-left: 5px !important;
}

@media (max-width: 768px) {
  .md-card {
    width: 100% !important;
  }
}
</style>
