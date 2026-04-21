<script lang="js" setup>
/**
 * DeveloperGreeting component
 *
 * @component
 * @name developer-greeting
 * @description
 * Greets a developer based on their name. If both names are empty, displays 'Welcome Anonymous Developer'.
 * Otherwise, congratulates the developer by their full name.
 *
 * @prop {string} firstName - The first name of the developer.
 * @prop {string} lastName - The last name of the developer.
 *
 * @example
 * <developer-greeting firstName="Ada" lastName="Lovelace" />
 * // Renders: Congrats Ada Lovelace! Now you are a Vue Developer
 *
 * <developer-greeting />
 * // Renders: Welcome Anonymous Developer
 */
import {Developer} from "../../domain/model/developer.entity.js";
import {computed} from "vue";

const props = defineProps(['firstName', 'lastName']);

/**
 * Computes the greeting message for the developer.
 * @type {import('vue').ComputedRef<string>}
 */
const greeting = computed(() => {
  if (!props.firstName?.trim() || !props.lastName?.trim()) {
    return "Welcome Anonymous Developer";
  }
  const developer = new Developer(props.firstName, props.lastName);
  return `Congrats ${developer.fullName}! Now you are a Vue Developer`;
});
</script>

<template>
  <p>{{ greeting }}</p>
</template>

<style scoped>
</style>