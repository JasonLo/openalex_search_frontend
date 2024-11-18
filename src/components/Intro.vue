<template>
  <div class="p-6 max-w-4xl mx-auto bg-white rounded-lg shadow-md mt-4">
    <h1 class="text-3xl font-bold mb-4">OpenAlex Search</h1>
    <p class="text-md mb-4">
      This application allows you to search for academic authors using Semantic Search and Reranker.
    </p>
    <h2 class="text-2xl font-semibold mb-2">How It Works</h2>
    <p class="text-md mb-4">
      When you enter a search query, our system processes the query and retrieves relevant publications from a small set of test data from OpenAlex. Each publication is <strong>groupby</strong> author, then <strong>aggregate (rerank)</strong> by the count of relevant papers. We plan to implement a more sophisticated re-ranking algorithm in the future.
    </p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Intro',
});
</script>
