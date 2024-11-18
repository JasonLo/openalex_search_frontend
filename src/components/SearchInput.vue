<template>
    <div class="p-6 max-w-4xl mx-auto bg-white rounded-lg shadow-md mt-4 mt-4">
      <div class="mb-4">
        <label for="query" class="block text-sm font-medium text-gray-700">Query</label>
        <input
          type="text"
          id="query"
          v-model="localQuery"
          class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
        />
      </div>
      <div class="mb-4">
        <label for="top_k" class="block text-sm font-medium text-gray-700">
          Top K: {{ localTopK }}
        </label>
        <input
          type="range"
          id="top_k"
          v-model="localTopK"
          :min="1"
          :max="100"
          class="mt-1 block w-full"
        />
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref, watch } from 'vue';
  
  export default defineComponent({
    name: 'SearchInput',
    props: {
      modelValueQuery: {
        type: String,
        required: true,
      },
      modelValueTopK: {
        type: Number,
        default: 3,
      },
    },
    setup(props, { emit }) {
      const localQuery = ref(props.modelValueQuery);
      const localTopK = ref(props.modelValueTopK);
  
      watch(() => props.modelValueQuery, (newVal) => {
        localQuery.value = newVal;
      });
  
      watch(() => props.modelValueTopK, (newVal) => {
        localTopK.value = newVal;
      });
  
      watch(localQuery, (newVal) => {
        emit('update:modelValueQuery', newVal);
      });
  
      watch(localTopK, (newVal) => {
        emit('update:modelValueTopK', newVal);
      });
  
      return {
        localQuery,
        localTopK,
      };
    },
  });
  </script>
  