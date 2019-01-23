<template>

  <!-- Component created to be a small and reusable simple html input.
  A few things can be selected: the type of input, the message above the input,
  the rules it has to validate, its placeholder and its passing validation
  message. However it won't work with more specific html input.
  A weird thing which appeared working on this component is that
  the v-validate attribute is bound to a variable without using 
  v-bind, using it does not work. -->

  <div class="form-group">
    <label :for="type">{{ msg }}</label>
    <input
      v-validate="rules"
      class="form-control"
      :class="{ 'is-invalid': slotProps.form.submitted && errors.has(type) }"
      :name="type"
      :type="type"
      :placeholder="placeholder"
      v-model="slotProps.form[type]"
    >
    <div class="valid-feedback">{{ validMsg }}</div>
    <div class="invalid-feedback">{{ errors.first(type) }}</div>
  </div>
</template>

<script>
export default {
  name: "CustomInput",
  props: {
    slotProps: {
      required: true
    },
    type: {
      type: String,
      required: true
    },
    msg: {
      type: String,
      required: true
    },
    rules: {
      type: String,
      required: false,
      default: ''
    
    },
    placeholder: {
      required: false,
      default: null
    },
    validMsg: {
      required: false,
      default: "Looks good!"
    }
  },
  data() {
    return {
    };
  },
  methods: {
  },

  /**
   * To be able to send the $validator of this component to the AddTodo
   * component I decided to send it to its parent the App component
   * on mount.
   */

  mounted() {
    this.$emit("mount", this.$validator);
  },
  computed: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>