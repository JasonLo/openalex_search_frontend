<template>
    <div class="container max-w-4xl mx-auto px-4">
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

        <!-- Status Messages -->
        <div v-if="loading" class="my-4">
            <p>Loading...</p>
        </div>
        <div v-if="errorMessage" class="my-4 text-red-500">
            <p>{{ errorMessage }}</p>
        </div>
        <div v-else-if="hasSearched && results.length === 0 && !loading" class="my-4">
            <p>No results found.</p>
        </div>

        <!-- Results Section -->
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
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import Intro from './Intro.vue';
import SearchInput from './SearchInput.vue';
import ResultCard from './ResultCard.vue';

export default defineComponent({
    name: 'SearchTool',
    components: {
        Intro,
        SearchInput,
        ResultCard,
    },
    setup() {
        // Reactive State
        const query = ref('Higgs boson');
        const topK = ref<number>(3);
        const results = ref<any[]>([]);
        const loading = ref(false);
        const errorMessage = ref('');
        const hasSearched = ref(false);

        // Helper Function to Build the API URL
        const buildSearchUrl = (query: string, topK: number): string => {
            const baseUrl = import.meta.env.VITE_OPENALEX_SEARCH_ENDPOINT;
            const params = new URLSearchParams({ query, top_k: topK.toString() });
            return `${baseUrl}?${params}`;
        };

        // Search Function
        const search = async () => {
            if (!query.value.trim()) {
                errorMessage.value = 'Please enter a search query.';
                return;
            }

            // Reset State
            errorMessage.value = '';
            results.value = [];
            loading.value = true;
            hasSearched.value = true;

            try {
                const response = await fetch(buildSearchUrl(query.value, topK.value));
                if (!response.ok) throw new Error(`Error: ${response.statusText}`);

                const data = await response.json();
                results.value = Array.isArray(data) ? data : [];
            } catch (error) {
                console.error(error);
                errorMessage.value = 'An error occurred while fetching results.';
            } finally {
                loading.value = false;
            }
        };

        // Return State and Methods
        return {
            query,
            topK,
            results,
            loading,
            errorMessage,
            hasSearched,
            search,
        };
    },
});
</script>
