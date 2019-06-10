<template>
  <v-app>
    <v-content>
      <TodoList/>
    </v-content>
  </v-app>
</template>

<script>
import TodoList from "@/components/ToDoList.vue";

export default {
  name: "App",
  components: {
    TodoList
  },
  data() {
    return {
      //
    };
  }
};
</script>
