<script lang="js" setup>
/**
 * App component
 *
 * @component
 * @name app
 * @description
 * The main application part. Manages developer registration, greeting, and count.
 * Handles events from DeveloperRegistration and updates state accordingly.
 *
 * @example
 * <app />
 */
import DeveloperRegistration from "./greetings/presentation/components/developer-registration.vue";
import DeveloperGreeting from "./greetings/presentation/components/developer-greeting.vue";
import DeveloperCountShow from "./greetings/presentation/components/developer-count-show.vue";
import {Developer} from "./greetings/domain/model/developer.entity.js";
import {ref} from "vue";

/**
 * The first name of the registered developer.
 * @type {import('vue').Ref<string>}
 */
const firstName = ref("");

/**
 * The last name of the registered developer.
 * @type {import('vue').Ref<string>}
 */
const lastName = ref("");

/**
 * The number of developers registered (excluding unknown developers).
 * @type {import('vue').Ref<number>}
 */
const developerCount = ref(0);

/**
 * Whether a developer has registered.
 * @type {import('vue').Ref<boolean>}
 */
const hasRegistered = ref(false);

/**
 * Handles the 'developer-registered' event. Updates developer info and count.
 * @param {{ firstName: string, lastName: string }} developer - The registered developer's info.
 * @returns {void}
 */
function updateRegisteredDeveloperInfo(developer) {
  firstName.value = developer.firstName;
  lastName.value = developer.lastName;
  hasRegistered.value = true;
  console.log("Developer registered: ", developer);
  updateDeveloperCount(developer);
}

/**
 * Handles the 'registration-deferred' event. Resets developer info.
 * @returns {void}
 */
function resetRegisteredDeveloperInfo() {
  firstName.value = "";
  lastName.value = "";
  hasRegistered.value = false;
  console.log("Registration deferred");
}

/**
 * Increments the developer count if the developer is not 'Unknown'.
 * @param {{ firstName: string, lastName: string }} developer - The developer to check.
 * @returns {void}
 */
function updateDeveloperCount(developer) {
  const dev = new Developer(developer.firstName, developer.lastName);
  if (dev.fullName !== "Unknown") {
    developerCount.value++;
  }
}
</script>

<template>
  <h1>Hello Vue Developer Application</h1>
  <developer-registration
      @developer-registered="updateRegisteredDeveloperInfo"
      @registration-deferred="resetRegisteredDeveloperInfo"
  />
  <developer-greeting
      v-if="hasRegistered"
      :first-name="firstName"
      :last-name="lastName"
  />
  <developer-count-show :developer-count="developerCount"/>
</template>

<style>
</style>