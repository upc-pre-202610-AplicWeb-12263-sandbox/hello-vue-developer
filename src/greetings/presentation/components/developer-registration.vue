<script setup>
/**
 doc * DeveloperRegistration component
 *
 * @component
 * @name developer-registration
 * @description
 * This component is responsible for registering a new developer. It provides buttons to register,
 * defer registration (later), or clear the form. It emits 'developer-registered' or 'registration-deferred' events
 * based on user actions. Shows feedback for invalid registration attempts.
 *
 * @example
 * <developer-registration @developer-registered="updateRegisteredDeveloperInfo" @registration-deferred="resetRegisteredDeveloperInfo" />
 */
import {ref} from 'vue';

/**
 * The first name input for the developer registration form.
 * @type {import('vue').Ref<string>}
 */
const firstName = ref("");

/**
 * The last name input for the developer registration form.
 * @type {import('vue').Ref<string>}
 */
const lastName = ref("");

/**
 * The error message to display for invalid registration attempts.
 * @type {import('vue').Ref<string>}
 */
const errorMessage = ref("");

/**
 * Emits events for registration actions.
 * The 'developer-registered' event is emitted with the developer's names when registration is successful.
 * The 'registration-deferred' event is emitted with empty names when the user chooses to defer registration.
 * @type {(event: 'developer-registered' | 'registration-deferred',
 * payload: { firstName: string, lastName: string }) => void}
 */
const emit = defineEmits(['developer-registered', 'registration-deferred']);

/**
 * Handles the registration form submission. Emits 'developer-registered' if valid, otherwise sets an error message.
 * @returns {void}
 */
function submitRegistrationRequest() {
  let submittedFirstName = firstName.value.toString().trim();
  let submittedLastName = lastName.value.toString().trim();
  if (submittedFirstName || submittedLastName) {
    console.log("Registering developer");
    emit("developer-registered", {
      firstName: submittedFirstName,
      lastName: submittedLastName
    });
    clearFields();
    errorMessage.value = "";
  } else {
    console.log("Cannot register: Both fields are required");
    errorMessage.value = "Please provide at least first name or last name. Warning: Both are required for a successful registration.";
  }
}

/**
 * Defers the registration process by emitting 'registration-deferred' event with empty names. Clears the form and error message.
 * @returns {void}
 */
function deferRegistration() {
  console.log("Deferring registration");
  emit("registration-deferred", {firstName: "", lastName: ""});
  clearFields();
  errorMessage.value = "";
}

/**
 * Clears the input fields and error message.
 * @returns {void}
 */
function clearFields() {
  firstName.value = "";
  lastName.value = "";
  errorMessage.value = "";
}
</script>

<template>
  <!-- Developer Registration Form -->
  <div>
    <h2>New Developer</h2>
    <div>
      <form v-on:submit.prevent="submitRegistrationRequest">
        <div class="field">
          <label for="firstName">First Name:</label><input id="firstName" v-model="firstName" type="text"/>
        </div>
        <div class="field">
          <label for="lastName">Last Name:</label><input id="lastName" v-model="lastName" type="text"/>
        </div>
        <div class="actions">
          <button type="submit">Register</button>
          <button type="button" v-on:click="deferRegistration">Later</button>
          <button type="button" v-on:click="clearFields">Clear</button>
        </div>
      </form>
      <p v-if="errorMessage" class="error" role="alert">{{ errorMessage }}</p>
    </div>
  </div>
</template>

<style scoped>
button {
  margin-right: 10px;
  padding: 8px 16px;
  cursor: pointer;
}

.error {
  color: #d32f2f;
  margin-top: 10px;
  font-size: 14px;
}

.field {
  margin-bottom: 10px;
}

.actions {
  margin-top: 10px;
}

label {
  margin-right: 5px;
}
</style>