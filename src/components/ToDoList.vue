<template>
  <div class="todo">
    <h1>Todos</h1>

    <ul v-if="todos">
      <li v-for="todo in todos">
        {{todo}}
        <span>
          <button @click.prevent="deleteTodo($event)">delete</button>
        </span>
      </li>
    </ul>
    <form ref="form" @submit.prevent="addTodo">
      <input type="text" v-model="todo" @click.prevent="addTodo">
    </form>

    <button>New Todo</button>
  </div>
</template>

<script>
//TODO: make todo object (id selfincreasing)
//TODO: save todos to localstorage
//TODO: todo object thingy
//TODO: pass down to child component
//TODO: addbutton doesn't work
//TODO: style (add bootstrap or vuetify)
//TODO: CRUD functionality
import ToDoItem from "@/components/ToDoItem.vue";

export default {
  data() {
    return {
      todos: [],
      todo: "".trim()
    };
  },
  components: {
    ToDoItem
  },
  methods: {
    addTodo: function() {
      //take user input and add to list->
      const todo = this.todo;
      if (todo.length) {
        this.todos.push(todo);
        //localStorage.setItem(todos, [todo]);
      }
      this.$refs.form.reset();
    },
    deleteTodo: function(e) {
      //TODO: find a more elegant solution:)
      e.target.parentElement.parentElement.remove();
    }
  }
};
</script>

<style scoped>
</style>
