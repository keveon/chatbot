<script lang="ts">
    import type { Command } from "../lib/commands";

    export let command: Command;

    let copied = false;

    const permissionMap = {
        admin: {
            label: "超管",
            class: "bg-gradient-to-r from-rose-500 to-pink-500 dark:from-rose-600 dark:to-pink-700 text-white",
        },
        manager: {
            label: "群管",
            class: "bg-gradient-to-r from-blue-500 to-cyan-500 dark:from-blue-600 dark:to-cyan-700 text-white",
        },
        all: {
            label: "所有人",
            class: "bg-gradient-to-r from-green-500 to-emerald-500 dark:from-green-600 dark:to-emerald-700 text-white",
        },
    };

    async function copyCommand() {
        try {
            await navigator.clipboard.writeText(command.command);
            copied = true;
            setTimeout(() => {
                copied = false;
            }, 2000);
        } catch (err) {
            console.error("复制失败:", err);
        }
    }
</script>

<div
    class="rounded-2xl border-2 border-gray-200 dark:border-gray-700 bg-white dark:bg-slate-800 overflow-hidden shadow-md hover:shadow-lg transition-all duration-300 hover:-translate-y-1"
>
    <div class="p-5">
        <div class="flex items-start justify-between mb-3">
            <h3 class="text-lg font-bold text-gray-800 dark:text-gray-100">
                {command.name}
            </h3>
            <span
                class="px-3 py-1 rounded-full {permissionMap[command.permission]
                    .class} text-xs font-medium shadow-sm"
            >
                {permissionMap[command.permission].label}
            </span>
        </div>

        <p
            class="text-sm text-gray-600 dark:text-gray-400 mb-3 leading-relaxed"
        >
            {command.description}
        </p>

        {#if command.warning}
            <div
                class="bg-gradient-to-r from-amber-50 to-orange-50 dark:from-amber-950 dark:to-orange-950 border-l-4 border-amber-500 mb-3 p-3 rounded-r-lg"
            >
                <div class="flex items-start gap-2">
                    <span class="text-lg">⚠️</span>
                    <span
                        class="text-xs text-amber-800 dark:text-amber-200 font-medium"
                        >{command.warning}</span
                    >
                </div>
            </div>
        {/if}

        {#if command.example}
            <div
                class="text-xs text-gray-500 dark:text-gray-400 mb-3 bg-gray-50 dark:bg-slate-700 p-2 rounded-lg"
            >
                💡 示例: <span
                    class="font-mono text-gray-700 dark:text-gray-300"
                    >{command.example}</span
                >
            </div>
        {/if}

        <button
            on:click={copyCommand}
            class="w-full py-3 px-4 rounded-xl font-medium transition-all relative overflow-hidden shadow-md hover:shadow-lg active:scale-95 text-white"
            class:bg-gradient-to-r={!copied}
            class:from-orange-500={!copied}
            class:to-pink-500={!copied}
            class:dark:from-orange-600={!copied}
            class:dark:to-pink-700={!copied}
            class:bg-green-500={copied}
            class:dark:bg-green-700={copied}
            disabled={copied}
        >
            {#if copied}
                <span class="flex items-center justify-center gap-2">
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
                            d="M5 13l4 4L19 7"
                        />
                    </svg>
                    已复制
                </span>
            {:else}
                <span class="flex items-center justify-center gap-2">
                    <code class="font-mono font-semibold"
                        >{command.command}</code
                    >
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-4 w-4"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"
                        />
                    </svg>
                </span>
            {/if}
        </button>
    </div>
</div>
