<template>
  <div class="tile columns">
    <div class="column tile">
      <div class="tile is-child box">
        <h2 class="title">Example:</h2>
        <Input
          label="Username"
          icon="fa-user"
          v-model="username.value"
          :ref="username.ref"
          :is-danger="!!username.error"
          :error-message="username.error && username.error.message"
        />
        <Input
          label="Password"
          type="password"
          icon="fa-key"
          v-model="password.value"
          :ref="password.ref"
          :is-danger="!!password.error"
          :error-message="password.error && password.error.message"
        />
        <Input
          label="Confirm Password"
          type="password"
          icon="fa-key"
          v-model="confirmedPassword.value"
          :ref="confirmedPassword.ref"
          :is-danger="!!confirmedPassword.error"
          :error-message="
            confirmedPassword.error && confirmedPassword.error.message
          "
        />
        <button class="button is-link" @click="onSubmit">Submit</button>
      </div>
    </div>
    <div class="column tile">
      <div class="tile is-child box">
        <h2 class="title">Values:</h2>
        <pre>{{ JSON.stringify(values, null, 2) }}</pre>
      </div>
    </div>
    <div class="column tile">
      <div class="tile is-child box">
        <h2 class="title">Errors:</h2>
        <pre>{{ JSON.stringify(errors, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import Input from "./components/Input.vue";
import { useForm } from "vue-hooks-form";

export default defineComponent({
  name: "Demo",
  components: {
    Input,
  },
  props: {
    modelValue: String,
  },
  setup() {
    const { useField, errors, values, handleSubmit } = useForm({
      defaultValues: {
        username: "Victor",
      },
    });
    const username = useField("username", {
      rule: { required: true },
    });
    const password = useField("password", {
      rule: {
        required: true,
        min: 6,
        max: 10,
      },
    });
    const confirmedPassword = useField("confirmedPassword", {
      rule: {
        required: true,
        validator(rule, value) {
          if (!value || value !== values.password) {
            return new Error(
              "The two passwords that you entered do not match!"
            );
          }
          return true;
        },
      },
    });
    const onSubmit = (data) => console.log(data);
    return {
      username,
      password,
      confirmedPassword,
      onSubmit: handleSubmit(onSubmit),
      errors,
      values,
    };
  },
});
</script>

<style scoped>
h1 {
  text-align: center;
}
.box {
  min-width: 250px;
}
</style>
