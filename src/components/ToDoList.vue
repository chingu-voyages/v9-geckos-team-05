<template>
  <v-layout>
    <v-card class="todos pa-2" v-show="display">
      <v-layout column align-center>
        <v-flex my-4 xs12>
          <h1>Todos</h1>
        </v-flex>
        <v-flex v-if="!todos.length">
          <p>No todos yet!</p>
        </v-flex>

        <v-flex xs12 class="todo stretch">
          <v-list v-if="todos.length" dense>
            <v-list-tile class="pa-0" v-for="todo in todos" :key="todo.id">
              <v-list-tile-action>
                <v-checkbox @change="completedTodo(todo)" v-model="todo.done"></v-checkbox>
              </v-list-tile-action>
              <v-list-tile-content :class="{ done: todo.done }">{{todo.title}}</v-list-tile-content>
              <v-spacer></v-spacer>

              <v-list-tile-action>
                <v-btn small flat fab @click.prevent="removeTodo(todo)">
                  <v-icon>delete</v-icon>
                </v-btn>
              </v-list-tile-action>
            </v-list-tile>
          </v-list>
        </v-flex>
        <v-flex xs12 class="stretch" px-4>
          <v-form ref="form" class="form" @submit.prevent="addTodo">
            <v-text-field
              input="text"
              autofocus
              v-model.trim="newTodo"
              placeholder="add todo"
              append-icon="close"
              @click:append="$refs.form.reset()"
            ></v-text-field>
          </v-form>
        </v-flex>
      </v-layout>
    </v-card>
    <v-layout justify-center>
      <v-flex class="toggle-btn">
        <v-btn flat @click.prevent="display = !display">Todo</v-btn>
      </v-flex>
    </v-layout>
  </v-layout>
</template>

<script>
//TODO: style -clean css please
//TODO: implement child component
//TODO: refactor
//TODO: test

"use strict";
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
      error: "",
      display: false
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
  width: 350px;
  border: 1px solid pink;
  position: absolute;
  right: 10px;
  bottom: 50px;
  overflow-y: scroll;
  max-height: 400px;
}

.stretch {
  width: 100%;
}

.done {
  text-decoration: line-through;
}

.toggle-btn {
  position: absolute;
  right: 0;
  bottom: 0;
}
</style>
