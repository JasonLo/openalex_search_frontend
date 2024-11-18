<template>
  <div class="bg-white shadow-md rounded-lg overflow-hidden">
    <div class="p-4">
      <div v-if="result.display_name" class="mb-2">
        <strong class="capitalize">{{ result.display_name }}</strong>
        <a> |</a>
        <a
          :href="result.id"
          target="_blank"
          class="text-blue-500 hover:text-blue-700 transition-colors duration-300"
        >
          Open in OpenAlex
        </a>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'ResultCard',
  props: {
    result: {
      type: Object,
      required: true,
    },
  },
});
</script>