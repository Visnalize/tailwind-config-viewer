<template>
  <button
    class="dark-hover:bg-gray-800 hover:bg-gray-300 px-4 py-2 border border-gray-400 dark:border-gray-700 rounded focus:outline-none text-gray-800 dark:text-gray-400 text-sm"
    :class="{
      'bg-gray-300 dark:bg-gray-800': selected,
      'bg-white dark:bg-gray-900': !selected
    }"
    v-on="$listeners"
  >
    <slot />
  </button>
</template>

<script>
export default {
  props: {
    selected: {
      type: Boolean
    }
  }
};
</script>
