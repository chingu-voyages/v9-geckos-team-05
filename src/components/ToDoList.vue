<template>
  <div class="todo">
    <h1>Do this!</h1>
    <button>New Todo</button>
    <input type="text" v-model="todo" @click.prevent="addTodo">
    <ul v-for="todo in todos">
      <li>{{todo}}</li>
    </ul>
  </div>
</template>

<script>
import ToDoItem from "@/components/ToDoItem.vue";

export default {
  data() {
    return {
      todos: ["nachos", "cars"],
      todo: ""
    };
  },
  components: {
    ToDoItem
  },
  methods: {
    addTodo: () => {
      //take user input and add to list->

      console.log(this.todos);
    }
  }
};
</script>

<style scoped>
</style>
