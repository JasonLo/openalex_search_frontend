<template>
  <div id="app" class="min-h-screen bg-gray-100 flex items-center justify-center">
    <SearchTool />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import SearchTool from './components/SearchTool.vue';

export default defineComponent({
  name: 'App',
  components: {
    SearchTool,
  },
});
</script>