<template>
  <div>
    <div v-for="(_, category) in data" :key="category">
      <div class="mb-4 text-gray-900 dark:text-gray-500 text-xl">
        {{ category }}
      </div>
      <div class="flex flex-wrap md:gap-4">
        <div
          v-for="(value, prop) in data[category]"
          :key="prop"
          class="flex flex-col mb-4 w-full md:w-64"
        >
          <code
            class="flex flex-col bg-gray-200 dark:bg-gray-800 p-2 w-full h-full text-gray-900 dark:text-gray-500"
          >
            <span v-for="(v, k) in value" :key="k">- {{ k }}: {{ v }}</span>
          </code>
          <CanvasBlockLabel :label="prop.slice(1)" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CanvasBlockLabel from '../CanvasBlockLabel';

export default {
  components: {
    CanvasBlockLabel
  },

  props: {
    data: {
      type: Object,
      required: true
    }
  }
};
</script>
