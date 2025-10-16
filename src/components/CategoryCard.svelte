<script lang="ts">
    import type { Category } from "../lib/commands";

    export let category: Category;

    // 为每个分类定义不同的渐变色
    const colorThemes: Record<
        string,
        { gradient: string; border: string; iconBg: string; hoverBg: string }
    > = {
        member: {
            gradient:
                "from-blue-50 to-cyan-50 dark:from-slate-800 dark:to-slate-800",
            border: "border-blue-200 dark:border-slate-600",
            iconBg: "bg-gradient-to-br from-blue-400 to-cyan-500 dark:from-blue-600 dark:to-cyan-700",
            hoverBg:
                "hover:from-blue-100 hover:to-cyan-100 dark:hover:from-slate-700 dark:hover:to-slate-700",
        },
        currency: {
            gradient:
                "from-amber-50 to-yellow-50 dark:from-slate-800 dark:to-slate-800",
            border: "border-amber-200 dark:border-slate-600",
            iconBg: "bg-gradient-to-br from-amber-400 to-yellow-500 dark:from-amber-600 dark:to-yellow-700",
            hoverBg:
                "hover:from-amber-100 hover:to-yellow-100 dark:hover:from-slate-700 dark:hover:to-slate-700",
        },
        ranking: {
            gradient:
                "from-purple-50 to-pink-50 dark:from-slate-800 dark:to-slate-800",
            border: "border-purple-200 dark:border-slate-600",
            iconBg: "bg-gradient-to-br from-purple-400 to-pink-500 dark:from-purple-600 dark:to-pink-700",
            hoverBg:
                "hover:from-purple-100 hover:to-pink-100 dark:hover:from-slate-700 dark:hover:to-slate-700",
        },
        features: {
            gradient:
                "from-green-50 to-emerald-50 dark:from-slate-800 dark:to-slate-800",
            border: "border-green-200 dark:border-slate-600",
            iconBg: "bg-gradient-to-br from-green-400 to-emerald-500 dark:from-green-600 dark:to-emerald-700",
            hoverBg:
                "hover:from-green-100 hover:to-emerald-100 dark:hover:from-slate-700 dark:hover:to-slate-700",
        },
        system: {
            gradient:
                "from-rose-50 to-orange-50 dark:from-slate-800 dark:to-slate-800",
            border: "border-rose-200 dark:border-slate-600",
            iconBg: "bg-gradient-to-br from-rose-400 to-orange-500 dark:from-rose-600 dark:to-orange-700",
            hoverBg:
                "hover:from-rose-100 hover:to-orange-100 dark:hover:from-slate-700 dark:hover:to-slate-700",
        },
    };

    const theme = colorThemes[category.id] || colorThemes.member;
</script>

<a
    href={`/category/${category.id}`}
    class="block p-6 rounded-2xl border-2 bg-gradient-to-br {theme.gradient} {theme.border} {theme.hoverBg} shadow-lg hover:shadow-xl transition-all duration-300 hover:scale-[1.02] active:scale-[0.98] group"
>
    <div class="flex items-start gap-4">
        <div
            class="p-3 rounded-xl {theme.iconBg} shadow-md group-hover:scale-110 transition-transform duration-300"
        >
            <span class="text-3xl block filter drop-shadow-sm"
                >{category.icon}</span
            >
        </div>
        <div class="flex-1">
            <h2
                class="text-xl font-bold mb-2 text-gray-800 dark:text-gray-100 group-hover:text-gray-900 dark:group-hover:text-white transition-colors"
            >
                {category.name}
            </h2>
            <p class="text-sm text-gray-600 dark:text-gray-400 leading-relaxed">
                {category.description}
            </p>
        </div>
    </div>

    <div
        class="flex items-center justify-between text-sm mt-4 pt-4 border-t border-gray-200 dark:border-gray-700"
    >
        <span class="text-gray-600 dark:text-gray-400 font-medium">
            共 <span class="font-bold text-gray-800 dark:text-gray-200"
                >{category.commands.length}</span
            > 条指令
        </span>
        <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5 text-gray-400 group-hover:text-gray-600 dark:group-hover:text-gray-300 group-hover:translate-x-1 transition-all"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
        >
            <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9 5l7 7-7 7"
            ></path>
        </svg>
    </div>
</a>
