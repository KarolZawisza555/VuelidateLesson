<template>
  <div class="container">
    <div>
      <p>Name</p>
      <input v-model="state.name" type="text" />
      <p>Email</p>
      <input v-model="state.email" type="text" />
      <p>Password</p>
      <input v-model="state.password.password" type="password" />
      <p>Password confirm</p>
      <input v-model="state.password.confirm" type="password" />
    </div>
    <div style="margin-top: 40px">
      <button class="clear" @click="clear">Clear</button>
      <button @click="reset">Reset</button>
      <button @click="showCode = !showCode">
        {{ showCode ? "Hide" : "Show " }} validation object
      </button>
    </div>
    <div class="container">
      <Transition
        name="custom-classes"
        enter-active-class="animate__animated animate__bounceInLeft"
        leave-active-class="animate__animated animate__bounceOutRight"
      >
        <table v-if="v$.$silentErrors.length">
          <thead>
            <tr>
              <th width="20%">Name</th>
              <th width="20%">Condition</th>
              <th width="40%">Message</th>
            </tr>
          </thead>
          <TransitionGroup
            tag="tbody"
            enter-active-class="animate__animated animate__bounceIn"
            leave-active-class="animate__animated animate__bounceOut"
          >
            <tr v-for="item in v$.$silentErrors" :key="item.$propertyPath">
              <td>{{ item.$propertyPath }}</td>
              <td>{{ item.$validator }}</td>
              <td>{{ item.$message }}</td>
            </tr>
          </TransitionGroup>
        </table>
      </Transition>
    </div>
    <Transition
      name="custom-classes"
      enter-active-class="animate__animated animate__bounceInLeft"
      leave-active-class="animate__animated animate__bounceOutRight"
    >
      <div v-if="showCode">
        <pre>
          <code>
            {{ v$ }}
          </code>
        </pre>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { reactive, ref, onMounted } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { required, email, sameAs } from "@vuelidate/validators";

const showCode = ref(false);
const state = reactive({
  name: "karl",
  email: "karl@kar.gov",
  password: { password: "12345", confirm: "12345" },
});
const rules = {
  name: { required },
  email: { required, email },
  password: {
    password: { required },
    confirm: { required, sameAsPassword: sameAs(state.password.password) },
  },
};
const clear = () => {
  state.name = "";
  state.email = "";
  state.password.password = "";
  state.password.confirm = "";
};
const reset = () => {
  state.name = "karl";
  state.email = "karl@kar.gov";
  state.password.password = "12345";
  state.password.confirm = "12345";
};
const v$ = useVuelidate(rules, state);
</script>
<style scoped>
.slide-fade-enter-active {
  transition: transform 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    opacity 0.3s ease-in-out;
}

.slide-fade-leave-active {
  transition: transform 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    opacity 0.3s ease-in-out;
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: transform 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94),
    opacity 0.3s ease-in-out;
}
</style>
