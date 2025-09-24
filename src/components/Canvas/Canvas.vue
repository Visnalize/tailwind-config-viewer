<template>
  <div class="bg-gray-100 dark:bg-midnight">
    <div
      v-if="!config"
      style="height: calc(100vh - 63px);"
      class="flex justify-center items-center"
    >
      <p class="font-bold text-gray-600 text-center">Loading Config...</p>
    </div>
    <div v-else class="flex px-3 pt-8">
      <div
        class="hidden md:block top-0 sticky flex-none pt-2 w-1/6 h-full max-h-screen overflow-y-auto"
      >
        <ToggleSwitch
          name="dark-mode"
          class="mb-3 ml-3"
          :value="darkMode"
          @input="$emit('toggle-dark-mode', $event)"
          label="Dark Mode"
        />
        <div class="ml-3 text-gray-700 dark:text-gray-500 text-sm">
          Tailwind
          <a
            :href="docsUrl"
            target="_blank"
            class="text-blue-700 dark:text-blue-400 underline hover:no-underline"
            >v{{ config.tailwindVersion }}</a
          >
        </div>
        <nav class="px-3 pt-3 pb-12 h-full">
          <a
            v-for="section in configTransformed"
            :key="section.title"
            :href="`#${section.title}`"
            class="relative flex items-center py-2 rounded-sm dark-hover:text-gray-200 hover:text-gray-900 text-base"
            :class="[
              activeSection === section
                ? 'text-gray-900 dark:text-gray-200'
                : 'text-gray-700 dark:text-gray-500'
            ]"
            @click="setActiveSection(section)"
          >
            <div
              class="absolute bg-gray-500 dark:bg-gray-600 rounded-full transition duration-200"
              :class="[
                activeSection === section
                  ? 'visible opacity-100'
                  : 'invisible opacity-0'
              ]"
              :style="{ width: '5px', height: '5px', left: '-12px' }"
            />
            {{ section.title }}
          </a>
        </nav>
      </div>
      <div class="md:pl-4 w-full md:w-5/6">
        <CanvasSection
          v-for="section in configTransformed"
          :key="section.title"
          :title="section.title"
          :id="section.title"
        >
          <Intersect
            :threshold="[0.0]"
            rootMargin="-40% 0px -60% 0px"
            @enter="setActiveSection(section)"
            @leaave="setActiveSection(null)"
          >
            <component
              :is="sectionComponent(section.component)"
              :data="section.data"
              :config="config"
            />
          </Intersect>
        </CanvasSection>
      </div>
    </div>
  </div>
</template>

<script>
import defu from 'defu';
import Intersect from 'vue-intersect';
import themeComponentMapper from './themeComponentMapper';
import fontTagCreator from './fontTagCreator';
import CanvasSection from './CanvasSection.vue';
import ToggleSwitch from '../ToggleSwitch.vue';
import defaultOptions from '../../defaultOptions';

export default {
  components: {
    CanvasSection,
    ToggleSwitch,
    Intersect
  },

  /** @returns {any} */
  provide() {
    return {
      prefixClassName: this.prefixClassName,
      getConfig: this.getConfig
    };
  },

  props: {
    darkMode: {
      type: Boolean,
      required: false
    }
  },

  data() {
    return {
      activeSection: null,
      config: null,
      configTransformed: null
    };
  },

  computed: {
    /** @returns {number} */
    tailwindMajorVersion() {
      return parseInt(this.config.tailwindVersion.split('.')[0], 10);
    },

    /** @returns {string} */
    docsUrl() {
      return this.tailwindMajorVersion < 4
        ? `https://v${this.tailwindMajorVersion}.tailwindcss.com/docs`
        : 'https://tailwindcss.com/docs';
    }
  },

  methods: {
    sectionComponent(component) {
      return require(`./Sections/${component}.vue`).default;
    },

    prefixClassName(className) {
      return this.config.prefix
        ? `${this.config.prefix}${className}`
        : className;
    },

    getConfig() {
      return this.config;
    },

    setActiveSection(section) {
      this.activeSection = section;
    }
  },

  async mounted() {
    const config = await fetch(window.__TCV_CONFIG.configPath);
    this.config = await config.json();
    this.config = defu(this.config, defaultOptions);
    this.configTransformed = themeComponentMapper(this.config.theme);
    fontTagCreator(this.config.theme);
  }
};
</script>
