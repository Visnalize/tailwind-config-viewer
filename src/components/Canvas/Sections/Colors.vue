<template>
  <div>
    <StickySectionHeader id="section-colors">
      <ButtonGroup>
        <Button
          class="w-full sm:w-32"
          :selected="selectedProp === 'backgroundColor'"
          @click="selectedProp = 'backgroundColor'"
        >
          Background
        </Button>
        <Button
          class="w-full sm:w-32"
          :selected="selectedProp === 'textColor'"
          @click="selectedProp = 'textColor'"
        >
          Text
        </Button>
        <Button
          class="w-full sm:w-32"
          :selected="selectedProp === 'borderColor'"
          @click="selectedProp = 'borderColor'"
        >
          Border
        </Button>
      </ButtonGroup>
    </StickySectionHeader>
    <div class="flex flex-wrap mt-6 -mb-4">
      <div
        v-for="(value, prop) in selectedColorItems"
        :key="prop"
        class="md:mr-4 mb-4 w-full md:w-36"
      >
        <div
          class="flex flex-none justify-center items-center mb-2 w-full md:w-36 h-16"
          :class="{ 'border border-gray-300': selectedProp === 'textColor' }"
          :style="tileStyle(value)"
        >
          <span
            class="text-3xl"
            :style="{
              color: value
            }"
            v-if="selectedProp === 'textColor'"
            >Aa</span
          >
        </div>
        <CanvasBlockLabel
          :label="`${selectedPropClassPrefix}-${prop}`"
          :value="value"
        />
      </div>
    </div>
  </div>
</template>

<script>
import CanvasBlockLabel from '../CanvasBlockLabel';
import ButtonGroup from '../../ButtonGroup';
import Button from '../../Button';
import StickySectionHeader from '../../StickySectionHeader';

export default {
  components: {
    CanvasBlockLabel,
    ButtonGroup,
    Button,
    StickySectionHeader
  },

  props: {
    data: {
      type: Object,
      required: true
    }
  },

  data() {
    return {
      selectedProp: 'backgroundColor'
    };
  },

  computed: {
    selectedColorItems() {
      return this.data[this.selectedProp];
    },

    selectedPropClassPrefix() {
      const map = {
        backgroundColor: 'bg',
        textColor: 'text',
        borderColor: 'border'
      };

      return map[this.selectedProp];
    }
  },

  methods: {
    tileStyle(value) {
      if (this.selectedProp === 'backgroundColor') {
        return {
          backgroundColor: value
        };
      }

      if (this.selectedProp === 'borderColor') {
        return {
          border: `2px solid ${value}`
        };
      }
    }
  }
};
</script>
