<template>
  <v-layout column align-center class="todos">
    <v-flex my-4>
      <h1>Todos</h1>
    </v-flex>
    <v-flex>
      <v-snackbar v-model="error" top :timeout="timeout">
        {{ error }}
        <v-btn color="pink" flat @click="snackbar = false">Close</v-btn>
      </v-snackbar>
    </v-flex>
    <v-flex my-4>
      <v-form ref="form" class="form" @submit.prevent="addTodo">
        <v-text-field
          input="text"
          solo
          autofocus
          v-model.trim="newTodo"
          placeholder="add todo"
          append-icon="close"
          append-outer-icon="add"
          @click:append-outer.prevent="addTodo"
          @click:append="$refs.form.reset()"
        ></v-text-field>
      </v-form>
    </v-flex>
    <v-flex>
      <v-list v-if="todos.length">
        <v-list-tile v-for="todo in todos" :key="todo.id">
          <v-list-tile-action>
            <v-checkbox
              @change="completedTodo(todo)"
              v-model="todo.done"
              :class="{ done: todo.done }"
              :label="todo.title"
            ></v-checkbox>
          </v-list-tile-action>

          <v-list-tile-action>
            <v-btn small flat fab @click.prevent="removeTodo(todo)">
              <v-icon>delete</v-icon>
            </v-btn>
          </v-list-tile-action>
        </v-list-tile>
      </v-list>
    </v-flex>
  </v-layout>
</template>

<script>
//TODO: style
//TODO: edit feature
//TODO: implement child component
//TODO: more thourough error handling

"use strict";
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
      done: false,
      error: "",
      snackbar: false,
      timeout: 4000
    };
  },
  methods: {
    addTodo: function() {
      try {
        if (this.newTodo.length) {
          this.todos.push({
            id: this.todos.length + 1,
            title: this.newTodo,
            done: false
          });

          localStorage.setItem("todos", JSON.stringify(this.todos));
        }
      } catch (error) {
        this.error = error.message;
        console.log(this.error);
      }
      this.$refs.form.reset();
    },

    completedTodo: function(todo) {
      if (todo.done === true) {
        localStorage.setItem("todos", JSON.stringify(this.todos));
      } else if (todo.done == false) {
        localStorage.setItem("todos", JSON.stringify(this.todos));
      }
    },
    removeTodo: function(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
      localStorage.setItem("todos", JSON.stringify(this.todos));
    }
  },
  created() {
    //TODO: refactor this
    const storageTodos = localStorage.getItem("todos");
    if (this.todos.length <= 0) {
      this.error = "No todos to do";
    }
    if (storageTodos) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    } else if (storageTodos === null || undefined) {
      localStorage.setItem("todos", JSON.stringify([]));
      this.error = "No todos to do";
    }
  }
};
</script>

<style scoped>
.todos {
  max-width: 400px;
}
.done {
  text-decoration: line-through;
}
</style>
