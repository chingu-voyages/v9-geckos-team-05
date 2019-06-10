<template>
  <v-layout column align-center>
    <v-flex my-4>
      <h1>Todos</h1>
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
            <v-checkbox @click="doneTodo(todo)"></v-checkbox>
          </v-list-tile-action>

          <v-list-tile-content>
            <v-list-tile-title>{{todo.title}}</v-list-tile-title>
          </v-list-tile-content>

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
//TODO: handle undefined error
//TODO: done strikethrough feature
//TODO: style
//TODO: max width and row
//TODO: display error messages in snackbar
//TODO: edit feature

"use strict";
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
      done: false,
      error: ""
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
    doneTodo: function(todo) {
      console.log(this.done);
    },
    removeTodo: function(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
      localStorage.setItem("todos", JSON.stringify(this.todos));
    }
  },
  created() {
    //TODO: print empty if empty
    this.todos = JSON.parse(localStorage.getItem("todos"));
  }
};
</script>

<style scoped>
.form {
  max-width: 400px;
}
</style>
