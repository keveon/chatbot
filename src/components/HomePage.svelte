<script lang="ts">
    import CategoryCard from "./CategoryCard.svelte";
    import CommandCard from "./CommandCard.svelte";
    import { categories } from "../lib/commands";
    import type { Command, Category } from "../lib/commands";

    let searchQuery = "";
    let searchResults: Array<{ command: Command; category: Category }> = [];
    let isSearching = false;

    $: {
        if (searchQuery.trim()) {
            performSearch(searchQuery);
            isSearching = true;
        } else {
            searchResults = [];
            isSearching = false;
        }
    }

    function performSearch(query: string) {
        const lowerQuery = query.toLowerCase().trim();
        const results: Array<{ command: Command; category: Category }> = [];

        categories.forEach((category) => {
            category.commands.forEach((command) => {
                const searchText =
                    `${command.name} ${command.command} ${command.description}`.toLowerCase();
                if (searchText.includes(lowerQuery)) {
                    results.push({ command, category });
                }
            });
        });

        searchResults = results;
    }

    function clearSearch() {
        searchQuery = "";
    }

    const quickSearchTerms = ["踢出", "金币", "排行榜", "开启"];
</script>

<div class="min-h-screen pb-20">
    <!-- Search Bar -->
    <div
        class="sticky top-16 z-10 bg-gradient-to-b from-amber-50 to-orange-50 dark:from-slate-900 dark:to-slate-900 px-4 py-4 shadow-md backdrop-blur-sm"
    >
        <div class="relative max-w-2xl mx-auto">
            <div
                class="absolute left-4 top-1/2 -translate-y-1/2 text-orange-400 dark:text-orange-500"
            >
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-5 w-5"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
                    />
                </svg>
            </div>
            <input
                bind:value={searchQuery}
                type="text"
                placeholder="搜索功能、指令..."
                class="w-full pl-12 pr-12 py-3 rounded-2xl border-2 border-orange-200 dark:border-slate-700 bg-white dark:bg-slate-800 text-gray-800 dark:text-gray-100 placeholder-gray-400 dark:placeholder-gray-500 focus:outline-none focus:border-orange-400 dark:focus:border-orange-700 shadow-sm transition-all"
            />
            {#if searchQuery}
                <button
                    on:click={clearSearch}
                    class="absolute right-4 top-1/2 -translate-y-1/2 text-gray-400 hover:text-orange-500 transition-colors"
                    aria-label="清除搜索"
                >
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-5 w-5"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M6 18L18 6M6 6l12 12"
                        />
                    </svg>
                </button>
            {/if}
        </div>
        {#if !isSearching}
            <div class="mt-3 text-center">
                <div class="flex flex-wrap gap-2 justify-center items-center">
                    <span
                        class="px-2 py-1 text-xs text-gray-600 dark:text-gray-400 font-medium"
                        >💡 试试</span
                    >
                    {#each quickSearchTerms as term}
                        <button
                            on:click={() => (searchQuery = term)}
                            class="px-3 py-1 text-xs rounded-full bg-white dark:bg-slate-700 border border-orange-200 dark:border-slate-600 text-orange-600 dark:text-orange-400 hover:bg-orange-50 dark:hover:bg-slate-600 transition-colors shadow-sm"
                        >
                            {term}
                        </button>
                    {/each}
                </div>
            </div>
        {/if}
    </div>

    <main class="container mx-auto px-4 py-6">
        {#if isSearching}
            <!-- Search Results -->
            {#if searchResults.length === 0}
                <div class="text-center py-12">
                    <div class="text-6xl mb-4">🔍</div>
                    <p class="text-lg text-gray-600 dark:text-gray-300">
                        未找到相关指令
                    </p>
                    <p class="text-sm text-gray-500 mt-2">试试其他关键词</p>
                </div>
            {:else}
                <div class="mb-4 text-sm text-gray-600 dark:text-gray-400">
                    找到 <span class="font-bold text-orange-500"
                        >{searchResults.length}</span
                    > 条相关指令
                </div>
                <div class="space-y-6">
                    {#each searchResults as { command, category }}
                        <div>
                            <div
                                class="text-xs text-gray-500 mb-2 flex items-center gap-2"
                            >
                                <span>{category.icon}</span>
                                <span>{category.name}</span>
                            </div>
                            <CommandCard {command} />
                        </div>
                    {/each}
                </div>
            {/if}
        {:else}
            <!-- Categories -->
            <section class="mb-6">
                <h2
                    class="text-2xl font-bold mb-6 px-2 bg-gradient-to-r from-orange-500 to-pink-500 bg-clip-text text-transparent"
                >
                    🎯 我想要...
                </h2>
                <div class="grid gap-4">
                    {#each categories as category}
                        <CategoryCard {category} />
                    {/each}
                </div>
            </section>
        {/if}
    </main>
</div>
