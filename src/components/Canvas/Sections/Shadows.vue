<template>
  <div class="flex flex-wrap -mb-4">
    <div
      v-for="(value, prop) in data"
      class="md:mr-8 mb-4 w-full md:w-36"
      :key="prop"
    >
      <div
        class="dark:bg-gray-700 mb-2 w-full md:w-36 h-16"
        :style="{
          boxShadow: value
        }"
      />
      <CanvasBlockLabel
        :label="`${removeDefaultSuffix(`shadow-${prop}`)}`"
        :value="value"
      />
    </div>
  </div>
</template>

<script>
import CanvasBlockLabel from '../CanvasBlockLabel';
import { removeDefaultSuffix } from '@/utils';

export default {
  components: {
    CanvasBlockLabel
  },

  props: {
    data: {
      type: Object,
      required: true
    }
  },

  methods: {
    removeDefaultSuffix
  }
};
</script>
