<template>

  <!-- Component created to allow the user to add things to do to the to-do list. It then evolved to be a more generic and reusable component but the default template to make the app works as a todo-app is still present inside a slot property and can easily be overriden. 
  A few slots are available even if one could have been enough, it aims to enable to easily structure the form -->

  <form id="form" class="needs-validation" novalidate @submit.prevent="onSubmit">
    <slot name="text" :form="form">
      <div class="form-group">
        <label for="newTodo">New todo:</label>
        <input
          v-validate="'required|alpha_spaces'"
          class="form-control"
          :class="{ 'is-invalid': form.submitted && errors.has('text') }"
          name="text"
          placeholder="Your next thing to do"
          v-model="form.text"
        >
        <div class="valid-feedback">Looks good!</div>
        <div class="invalid-feedback">{{ errors.first('text') }}</div>
      </div>
    </slot>
    <slot name="checkbox" :form="form"></slot>
    <slot name="radio" :form="form"></slot>
    <div class="form-row">
      <slot name="email" :form="form"></slot>
      <slot name="password" :form="form"></slot>
    </div>
    <div class="form-row">
      <slot name="date" :form="form"></slot>
      <slot name="color" :form="form"></slot>
    </div>
    <slot :form="form"></slot>
    <p>
      <button type="submit">
        <slot name="submit">Add to-do</slot>
      </button>
    </p>
  </form>
</template>

<script>
export default {
  name: "AddTodo",
  props: {
    pvalidators: {
      required: false
    }
  },
  data() {
    return {
      form: {
        checkbox: [],
        date: null,
        color: "#000000",
        email: null,
        password: null,
        radio: null,
        text: "",
        submitted: false
      }
    };
  },
  methods: {
    onSubmit() {
      this.form.submitted = true;

      /**
       * To prevent from emitting the data if one of the rules was broken I 
       * used the $validator API from vee-validate. However, each component 
       * has its own $validator and cannot handle all the rules of all the 
       * components. Therefore the solution I found was to agregate them all 
       * in one place and make sure they all pass before emitting anything. 
       * It could have been interesting to have this list of $validator in a 
       * centralized container. Vuex or another flux library would have been 
       * useful for this. Although I felt it was a bit overkill for the 
       * problem.
       */

      const validations = [];
      validations.push(this.$validator.validate());
      this.pvalidators.forEach(pvalidator => {
        validations.push(pvalidator.validate());
      });

      Promise.all(validations).then(results => {
        if (results.reduce((acc, curr) => acc && curr, true)) {
          this.$emit("add-todo-submitted", this.form);
          this.init();
          const matcher = {
            scope: null,
            vmId: this.$validator.id
          };

          this.$validator.reset(matcher);

          this.pvalidators.forEach(pvalidator => {
            const pmatcher = {
              scope: null,
              vmId: pvalidator.id
            };
            pvalidator.reset(pmatcher);
          });
        }
      });
    },
    init() {
      this.form = {
        checkbox: [],
        date: null,
        color: "#000000",
        email: "",
        password: null,
        radio: null,
        text: "",
        submitted: false
      };
    }
  },
  computed: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
