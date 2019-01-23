<template>

  <!-- This component aims to display all the to-do in the to-do list
  or the done in the done list. Behaviour changes whether the done prop
  is true or false. When a to-do is deleted or completed, an event is
  recieved and the list is updated. The list is directly updated in this
  component even if it receives it as a prop. It would have been interesting
  to commit a change to a state tree using vuex for example to modify this
  list. -->

  <div>
    <h1>{{ title }}</h1>
    <p v-if="!todos.length">{{ legend }}</p>
    <ul>
      <li v-for="(todo, index) in todos" v-bind:key="index">
        <Todo
          :text="todo.text"
          :key="index"
          :index="index"
          :done="done"
          @deletion="deleteOne"
          @todo-deleted="deleteTodo"
          @done-deleted="deleteDone"
        />
      </li>
    </ul>
    <button
      v-show="todos.length"
      v-on:click="deleteAllTodos"
      v-on:mouseenter="increaseOpacity"
      v-on:mouseout="decreaseOpacity"
    >
      <img
        src="../assets/garbage.png"
        v-on:mouseenter="increaseOpacity"
        v-on:mouseout="decreaseOpacity"
        alt="garbage"
        class="big"
      >
    </button>
  </div>
</template>

<script>
import Todo from "./Todo.vue";

export default {
  name: "TodoList",
  components: {
    Todo
  },
  props: {
    done: {
      type: Boolean,
      required: true
    },
    todos: {
      type: Array,
      required: false,
      default: function() {
        return [];
      }
    }
  },
  data() {
    return {
    };
  },
  methods: {
    deleteTodo(index) {
      this.$emit("todo-deleted", this.todos[index]);
      this.todos.splice(index, 1);
    },
    deleteDone(index) {
      this.$emit("done-deleted", this.todos[index]);
      this.todos.splice(index, 1);
    },
    deleteAllTodos() {
      this.$emit("all-deleted", this.done);
    },
    deleteOne(index) {
      this.todos.splice(index, 1);
    },
    increaseOpacity(e) {
      if (e.target.childNodes.length) e.target.childNodes[0].style.opacity = 1;
      else e.target.style.opacity = 1;
    },
    decreaseOpacity(e) {
      if (e.target.childNodes.length)
        e.target.childNodes[0].style.opacity = 0.6;
      else e.target.style.opacity = 0.6;
    }
  },
  computed: {
    title() {
      if (this.done) {
        return "My done list";
      } else {
        return "My to-do list";
      }
    },
    legend() {
      if (this.done) {
        return "No done yet";
      } else {
        return "No to-do yet";
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

img {
  width: 20px;
  height: 20px;
  opacity: 0.6;
}

.move {
  opacity: 0.3;
}

.big {
  width: 40px;
  height: 40px;
}

button {
  background-color: transparent;
  border: none;
}
</style>
