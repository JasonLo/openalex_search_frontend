<template>
    <div class="container mx-auto px-4">
        <Intro />
        <SearchInput 
            v-model:modelValueQuery="query" 
            :v-model:modelValueTopK="topK" 
        />
        <button
            @click="search"
            class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 mt-4"
        >
            Search
        </button>

        <div v-if="loading" class="my-4">
            <p>Loading...</p>
        </div>

        <div v-if="results.length > 0" class="my-8">
            <h2 class="text-xl font-semibold mb-4">Results:</h2>
            <div class="grid grid-cols-1 gap-2">
                <ResultCard
                    v-for="(result, index) in results"
                    :key="index"
                    :result="result"
                />
            </div>
        </div>

        <div v-else-if="searchInitiated && results.length === 0" class="my-4">
            <p>No results found.</p>
        </div>

        <div v-if="errorMessage" class="my-4 text-red-500">
            <p>{{ errorMessage }}</p>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import SearchInput from './SearchInput.vue';
import ResultCard from './ResultCard.vue';
import Intro from './Intro.vue';

export default defineComponent({
    name: 'SearchTool',
    components: {
        SearchInput,
        ResultCard,
        Intro,
    },
    setup() {
        const query = ref('higgs boson');
        const topK = ref<number>(3);
        const results = ref<any[]>([]);
        const loading = ref(false);
        const errorMessage = ref('');
        const searchInitiated = ref(false);

        const search = async () => {
            if (!query.value.trim()) {
                errorMessage.value = 'Please enter a search query.';
                return;
            }

            errorMessage.value = '';
            results.value = []; // Clear previous results
            searchInitiated.value = true;
            loading.value = true;

            try {
                const response = await fetch(buildSearchUrl(query.value, topK.value));
                if (!response.ok) {
                    throw new Error(`Error: ${response.statusText}`);
                }
                const data = await response.json();
                results.value = Array.isArray(data) ? data : []; // Ensure data is an array
            } catch (error) {
                console.error(error);
                errorMessage.value = 'An error occurred while fetching results.';
            } finally {
                loading.value = false;
            }
        };

        const buildSearchUrl = (query: string, topK: number) => {
            const params = new URLSearchParams({ query, top_k: topK.toString() });
            return `http://localhost:8000/search?${params}`;
        };

        return {
            query,
            topK,
            results,
            loading,
            errorMessage,
            searchInitiated,
            search,
        };
    },
});
</script>
