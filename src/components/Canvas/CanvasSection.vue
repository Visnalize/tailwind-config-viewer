<template>
  <section class="mb-12 max-w-full">
    <h1 class="mb-2 text-gray-800 dark:text-gray-500 text-3xl">{{ title }}</h1>
    <div
      class="bg-white dark:bg-gray-900 p-6 border border-gray-300 dark:border-gray-900 rounded"
    >
      <slot />
    </div>
  </section>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    }
  }
};
</script>
