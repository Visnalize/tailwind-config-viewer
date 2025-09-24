<template>
  <div>
    <div
      class="inline-block relative mr-2 w-10 align-middle transition duration-200 ease-in select-none"
    >
      <input
        v-model="checked"
        type="checkbox"
        :name="name"
        :id="name"
        class="block absolute bg-white border-4 rounded-full w-6 h-6 appearance-none cursor-pointer toggle-checkbox"
      />
      <label
        :for="name"
        class="block rounded-full h-6 overflow-hidden cursor-pointer toggle-label"
      ></label>
    </div>
    <label
      v-if="label"
      :for="name"
      class="text-gray-700 dark:text-gray-500 text-xs"
      >{{ label }}</label
    >
  </div>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true
    },

    value: {
      type: Boolean,
      default: false
    },

    label: {
      type: String,
      required: false
    }
  },

  data() {
    return {
      checked: this.value
    };
  },

  watch: {
    checked(newValue) {
      this.$emit('input', newValue);
    }
  }
};
</script>
<style lang="scss">
/* CHECKBOX TOGGLE SWITCH */
/* @apply rules for documentation, these do not work as inline style */
.toggle-checkbox:checked {
  @apply right-0 border-gray-500;
  right: 0;
}

.toggle-checkbox {
  right: calc(100% - theme('width.6'));
  transition: right;

  .mode-dark & {
    @apply border-gray-600;
  }

  .mode-dark &:checked {
    @apply border-gray-700;
  }

  &:focus {
    outline: none;
    box-shadow: 0 0 0 1px theme('colors.blue.300');
  }
}

.toggle-checkbox,
.toggle-label {
  @apply duration-100 ease-out;
}

.toggle-label {
  @apply bg-gray-300;

  .mode-dark & {
    @apply bg-gray-600;
  }
}

.toggle-checkbox:focus + .toggle-label {
}

.toggle-checkbox:checked + .toggle-label {
  @apply bg-gray-500;

  .mode-dark & {
    @apply bg-gray-700;
  }
}
</style>
