<script lang="ts">
    import { onMount } from 'svelte';

    // 主题模式：auto -> light -> dark -> auto
    let currentTheme: string = 'auto';

    // 组件挂载时初始化主题
    onMount(() => {
        currentTheme = localStorage.getItem('theme') || 'auto';
        applyTheme(currentTheme);
        updateThemeIcon();

        // 监听系统主题变化（仅在 auto 模式下生效）
        const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
        mediaQuery.addEventListener('change', (e) => {
            if (currentTheme === 'auto') {
                applyTheme('auto');
            }
        });
    });

    function applyTheme(theme: string) {
        const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;

        if (theme === 'auto') {
            // 自动模式：跟随系统
            if (prefersDark) {
                document.documentElement.classList.add('dark');
            } else {
                document.documentElement.classList.remove('dark');
            }
        } else if (theme === 'dark') {
            document.documentElement.classList.add('dark');
        } else {
            document.documentElement.classList.remove('dark');
        }
    }

    function updateThemeIcon() {
        // 这里通过 CSS 类来控制图标显示
        const root = document.documentElement;
        root.setAttribute('data-theme', currentTheme);
    }

    function toggleTheme() {
        // 循环切换：auto -> light -> dark -> auto
        if (currentTheme === 'auto') {
            currentTheme = 'light';
        } else if (currentTheme === 'light') {
            currentTheme = 'dark';
        } else {
            currentTheme = 'auto';
        }

        localStorage.setItem('theme', currentTheme);
        applyTheme(currentTheme);
        updateThemeIcon();
    }
</script>

<button
    on:click={toggleTheme}
    class="p-2 rounded-full hover:bg-orange-100 dark:hover:bg-slate-700 transition-colors relative group"
    aria-label="切换主题"
    title="切换主题"
>
    <!-- 浅色模式图标 -->
    <svg
        class="h-6 w-6 text-orange-500 {$$props.class || ''}"
        class:hidden={currentTheme !== 'light'}
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
    >
        <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"
        ></path>
    </svg>

    <!-- 深色模式图标 -->
    <svg
        class="h-6 w-6 text-orange-400 {$$props.class || ''}"
        class:hidden={currentTheme !== 'dark'}
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
    >
        <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
        ></path>
    </svg>

    <!-- 自动模式图标 -->
    <svg
        class="h-6 w-6 text-orange-500 {$$props.class || ''}"
        class:hidden={currentTheme !== 'auto'}
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
    >
        <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z"
        ></path>
    </svg>

    <!-- 提示文字 -->
    <span
        class="absolute -bottom-8 left-1/2 -translate-x-1/2 px-2 py-1 bg-gray-800 dark:bg-gray-700 text-white text-xs rounded opacity-0 group-hover:opacity-100 transition-opacity whitespace-nowrap pointer-events-none z-50"
    >
        {#if currentTheme === 'auto'}
            自动
        {:else if currentTheme === 'light'}
            浅色
        {:else}
            深色
        {/if}
    </span>
</button>

<style>
    /* 确保图标切换动画平滑 */
    svg {
        transition: opacity 0.2s ease-in-out;
    }
</style>
