<template>

  <!-- The App component is calling the different components of the apps.
  It calls the AddTodo component which will enable to add a to-do to our 
  to-do list which is created by the App component. The AddTodo aims to
  be as reusable as possible and to be able to be populated with the inputs
  we want so there is a few examples of pre built templates that can be
  used inside the AddTodo slots, just uncomment them to try them out. 
  -->

  <div id="app">

    <AddTodo :pvalidators="pvalidators" @add-todo-submitted="addTodo">

      <template slot="text" slot-scope="slotProps">
        <CustomInput
          :slotProps="slotProps"
          msg="What is my next thing to do?"
          type="text"
          rules="required"
          placeholder="Make a to-do app"
          @mount="addValidator"
        ></CustomInput>
      </template>

      <!-- <template slot="email" slot-scope="slotProps">
        <CustomInput
          :slotProps="slotProps"
          msg="Email:"
          type="email"
          rules="required|email"
          placeholder="foo.bar@example.com"
          @mount="addValidator"
        ></CustomInput>
      </template>-->

      <!-- <template slot="password" slot-scope="slotProps">
        <CustomInput
          :slotProps="slotProps"
          msg="Password:"
          type="password"
          rules="required|min:6"
          @mount="addValidator"
        ></CustomInput>
      </template>-->

      <!-- <template slot="radio" slot-scope="slotProps">
        <div class="form-group">
          <span>Select your gender</span>
          <br>
          <label class="radio">
            <input
              v-validate="'required'"
              type="radio"
              name="gender"
              value="male"
              checked
              v-model="slotProps.form.radio"
            > Male
          </label>
          <label class="radio">
            <input type="radio" name="gender" value="female" v-model="slotProps.form.radio"> Female
          </label>
          <label class="radio">
            <input type="radio" name="gender" value="other" v-model="slotProps.form.radio"> Not binary
          </label>
          <div class="help is-danger" v-show="errors.has('gender')">Please select a gender</div>
        </div>
      </template>-->

      <template slot="date" slot-scope="slotProps">
        <CustomInput
          :slotProps="slotProps"
          msg="Birthday:"
          type="date"
          @mount="addValidator"
        ></CustomInput>
      </template>

      <template slot="color" slot-scope="slotProps">
        <CustomInput
          :slotProps="slotProps"
          msg="Select your favorite color:"
          type="color"
          validMsg="Great choice!"
          @mount="addValidator"
        ></CustomInput>
      </template>

      <template slot="checkbox" slot-scope="slotProps">
        <div class="form-group">
          <input
            v-validate="'required'"
            name="checkbox"
            type="checkbox"
            id="commitment"
            value="Commitment"
            v-model="slotProps.form.checkbox"
          >
          <label for="commitment">I swear on my honour that I will do my to-dos.</label>
          <div class="help is-danger" v-show="errors.has('checkbox')">You have to swear!</div>
        </div>
      </template>

    </AddTodo>

    <!-- The App component then calls the TodoList component twice, one 
    providing it with the to-do list and the done prop as false and the 
    second with the done list and the done prop as true. I could also have 
    used a single list with a done property for each item defining in which 
    list it should have been rendered. -->

    <TodoList :done="false" :todos="todoList" @todo-deleted="addDone" @all-deleted="deleteAll"/>
    <TodoList :done="true" :todos="doneList" @done-deleted="addTodo" @all-deleted="deleteAll"/>
  
  </div>
</template>

<script>
import TodoList from "./components/TodoList.vue";
import AddTodo from "./components/AddTodo.vue";
import CustomInput from "./components/CustomInput";

export default {
  name: "app",
  components: {
    AddTodo,
    TodoList,
    CustomInput
  },
  data() {
    return {
      todoList: [],
      doneList: [],
      pvalidators: [this.$validator]
    };
  },
  methods: {
    addTodo(todo) {
      this.todoList.push({
        text: todo.text
      });
    },
    addDone(todo) {
      this.doneList.push(todo);
    },
    deleteAll(done) {
      if (done) {
        this.doneList = [];
      } else {
        this.todoList = [];
      }
    },
    addValidator(validator) {
      this.pvalidators.push(validator);
    }
  }
};
</script>

<style>
#app {
  display: flex;
  justify-content: space-around;
}

/* For some reason I couldn't manage to get the .invalid-feedback bootstrap
class working with checkbox and radio inputs so I made up a .is-danger class
as shown in the Vee validate documentation. */

.is-danger {
  width: 100%;
  margin-top: 0.25rem;
  font-size: 80%;
  color: #dc3545;
}
</style>
