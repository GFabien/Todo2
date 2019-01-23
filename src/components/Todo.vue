<template>

  <!-- This component aims to display a single to-do and provides different
  ways to interact with it: you can either delete it or mark it as completed
  if it wasn't done or mark it as still do to if it was complete. -->

  <div class="todo">
    <button
      v-on:click="deleteOne"
      v-on:mouseenter="increaseOpacity"
      v-on:mouseout="decreaseOpacity"
    >
      <img
        src="../assets/garbage.png"
        v-on:mouseenter="increaseOpacity"
        v-on:mouseout="decreaseOpacity"
        alt="remove"
      >
    </button>
    <button
      v-show="done"
      v-on:click="deleteDone"
      v-on:mouseenter="increaseOpacity"
      v-on:mouseout="decreaseOpacity"
    >
      <img
        src="../assets/cross.png"
        v-on:mouseenter="increaseOpacity"
        v-on:mouseout="decreaseOpacity"
        alt="cross"
        class="move"
      >
    </button>
    <button
      v-show="!done"
      v-on:click="deleteTodo"
      v-on:mouseenter="increaseOpacity"
      v-on:mouseout="decreaseOpacity"
    >
      <img
        src="../assets/tick.png"
        v-on:mouseenter="increaseOpacity"
        v-on:mouseout="decreaseOpacity"
        alt="tick"
        class="move"
      >
    </button>
    <p>{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: "Todo",
  props: {
    text: {
      type: String,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    done: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
    };
  },
  methods: {
    deleteTodo() {
      this.$emit("todo-deleted", this.index);
    },
    deleteDone() {
      this.$emit("done-deleted", this.index);
    },
    deleteOne() {
      this.$emit("deletion", this.index);
    },
    
    /**
     * I added two methods to modify the opacity of the pictures when hovering
     * over them instead of using css props because it wasn't working on Firefox.
     */

    increaseOpacity(e) {
      if (e.target.childNodes.length) e.target.childNodes[0].style.opacity = 1;
      else e.target.style.opacity = 1;
    },
    decreaseOpacity(e) {
      if (e.target.childNodes.length)
        e.target.childNodes[0].style.opacity = 0.3;
      else e.target.style.opacity = 0.3;
    }
  },
  computed: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.todo {
  display: flex;
}
img {
  width: 20px;
  height: 20px;
  opacity: 0.6;
}

.move {
  opacity: 0.3;
}
button {
  background-color: transparent;
  border: none;
}
</style>
