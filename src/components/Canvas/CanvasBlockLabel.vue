<template>
  <div class="relative">
    <div
      :class="{ show: showCopyTooltip }"
      class="z-50 absolute bg-gray-800 dark:bg-midnight shadow-md px-2 py-1 rounded overflow-hidden text-white text-sm transition duration-200 pointer-events-none tooltip"
    >
      {{ copyText }}
      <br />
      <div
        class="-mx-2 mt-1 px-2 py-1 border-gray-700 dark:border-gray-800 border-t"
        v-if="prefixedClassesToCopy.length > 1"
      >
        <div
          v-for="className in prefixedClassesToCopy"
          class="text-teal-400"
          :key="className"
        >
          {{ className }}
        </div>
      </div>
    </div>
    <input class="hidden" readonly ref="label" :value="copyValue" />
    <div
      class="inline-block font-mono text-gray-800 dark-hover:text-teal-400 hover:text-teal-600 dark:text-gray-400 text-sm break-words cursor-pointer"
      @click="copy"
      @mouseover="showCopy"
      @mouseout="hideCopy"
    >
      {{ prefixClassName(label) }}
    </div>
    <div
      v-if="value"
      class="text-gray-700 dark:text-gray-500 text-sm break-words"
    >
      {{ displayValue }}
    </div>
  </div>
</template>

<script>
import { appendPxToRems } from '@/utils';

let copyClasses = [];

export default {
  inject: ['prefixClassName', 'getConfig'],

  props: {
    label: {
      type: String,
      required: true
    },

    value: {
      type: String
    }
  },

  data() {
    return {
      showCopyTooltip: false,
      copyText: 'Copy',
      copyClasses: []
    };
  },

  computed: {
    /** @returns {string} */
    copyValue() {
      return this.prefixedClassesToCopy.join(' ');
    },

    /** @returns {string} */
    displayValue() {
      return appendPxToRems(this.value, this.getConfig());
    },

    /** @returns {string[]} */
    prefixedClassesToCopy() {
      return this.copyClasses.map(this.prefixClassName);
    }
  },

  methods: {
    copy(e) {
      if (e.shiftKey) {
        !copyClasses.includes(this.label) && copyClasses.push(this.label);
      } else {
        copyClasses = [this.label];
      }

      this.copyClasses = copyClasses;

      this.$nextTick(() => {
        // input needs to be visible in order for text to be selected/copied
        this.$refs.label.classList.remove('hidden');
        this.$refs.label.select();
        this.copyText =
          copyClasses.length > 1 ? `Copied (${copyClasses.length})` : 'Copied';
        document.execCommand('copy');
        this.$refs.label.blur();
        // hide input now that we copied the text to clipoard
        this.$refs.label.classList.add('hidden');
        window.getSelection().removeAllRanges();
      });
    },

    showCopy() {
      this.copyClasses = [];
      this.copyText = 'Copy';
      this.showCopyTooltip = true;
    },

    hideCopy() {
      this.showCopyTooltip = false;
    }
  }
};
</script>

<style scoped>
.tooltip {
  bottom: 100%;
  opacity: 0;
  transform: translateY(10px);
  visibility: hidden;
  transition: 0.15s ease-out;
}

.tooltip.show {
  opacity: 1;
  transform: translateY(0);
  visibility: visible;
}
</style>
