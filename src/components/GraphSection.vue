<template>
  <section v-if="selectedTicker" class="relative">
    <h3 class="text-lg leading-6 font-medium text-gray-900 my-8">
      {{ selectedTicker.name }} - USD
    </h3>
    <div class="flex items-end border-gray-600 border-b border-l h-64">
      <div
        v-for="(bar, idx) in normalizedGraph"
        :key="idx"
        :style="{ height: `${bar}%` }"
        class="bg-purple-800 border w-10"
      ></div>
    </div>
    <close-graph
      @click="$emit('close-graph')"
      type="button"
      class="absolute top-0 right-0"
    />
  </section>
</template>

<script>
import closeGraph from "./CloseGraph.vue";

export default {
  components: {
    closeGraph,
  },

  props: {
    selectedTicker: {
      type: Object,
      required: true,
    },

    normalizedGraph: {
      type: Array,
      required: true,
    },
  },
};
</script>
