<script setup>
import { computed } from 'vue';
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';

const props = defineProps({
    overview: {
        type: Object,
        default: () => ({
            rank: 8,
            totalScore: 7200,
            accuracy: 72,
            quizzesTaken: 5,
        }),
    },
    leaderboard: {
        type: Array,
        default: () => [
            { rank: 1, player: 'João Silva', score: 9200, time: '12:34', accuracy: 92 },
            { rank: 2, player: 'Maria Santos', score: 8950, time: '13:22', accuracy: 89 },
            { rank: 3, player: 'Carlos Oliveira', score: 8600, time: '14:15', accuracy: 86 },
            { rank: 4, player: 'Ana Costa', score: 8200, time: '15:45', accuracy: 82 },
            { rank: 5, player: 'Pedro Alves', score: 7800, time: '16:30', accuracy: 78 },
            { rank: 6, player: 'Lúcia Ferreira', score: 7500, time: '17:00', accuracy: 75 },
        ],
    },
});

const metricCards = computed(() => [
    {
        label: 'Sua posição',
        value: props.overview?.rank ? `#${props.overview.rank}` : '—',
        helper: 'Ranking global',
    },
    {
        label: 'Pontuação total',
        value: props.overview?.totalScore ? props.overview.totalScore.toLocaleString('pt-BR', { minimumFractionDigits: 0 }) : '—',
        helper: 'Pontos acumulados',
    },
    {
        label: 'Precisão',
        value: props.overview?.accuracy ? `${props.overview.accuracy}%` : '—',
        helper: 'Taxa de acertos',
    },
    {
        label: 'Quizzes jogados',
        value: props.overview?.quizzesTaken ?? '—',
        helper: 'Partidas concluídas',
    },
]);
</script>

<template>

    <Head title="Dashboard" />

    <AuthenticatedLayout>

        <div class="bg-gradient-to-b from-neutral-50 via-white to-white py-10">
            <div class="mx-auto max-w-6xl space-y-10 px-4 sm:px-6 lg:px-0">
                <div class="grid gap-4 md:grid-cols-2 xl:grid-cols-4">
                    <article v-for="card in metricCards" :key="card.label"
                        class="rounded-2xl border border-emerald-50 bg-white/95 p-6 shadow-lg shadow-emerald-50">
                        <p class="text-sm font-semibold uppercase tracking-wide text-slate-400">
                            {{ card.label }}
                        </p>
                        <p class="mt-3 text-3xl font-bold text-slate-900">
                            {{ card.value }}
                        </p>
                        <p class="mt-2 text-sm text-slate-500">{{ card.helper }}</p>
                    </article>
                </div>

                <div class="flex flex-wrap gap-4">
                    <button type="button"
                        class="rounded-button bg-emerald-500 px-6 py-3 text-sm font-semibold text-white shadow-lg shadow-emerald-200 transition hover:bg-emerald-600 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-emerald-500 focus-visible:ring-offset-2">
                        Iniciar novo quiz
                    </button>
                    <button type="button"
                        class="rounded-button border border-emerald-200 px-6 py-3 text-sm font-semibold text-emerald-600 transition hover:bg-emerald-50 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-emerald-200 focus-visible:ring-offset-2">
                        Ver histórico
                    </button>
                </div>

                <section class="rounded-3xl border border-slate-100 bg-white/95 p-6 shadow-xl">
                    <header class="mb-6 flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <div
                                class="flex h-10 w-10 items-center justify-center rounded-2xl bg-emerald-100 text-emerald-600">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"
                                    fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                    stroke-linejoin="round">
                                    <path d="m7 21 5-5 5 5" />
                                    <path d="m7 4 5 5 5-5" />
                                    <path d="M17 10H7" />
                                    <path d="M17 14H7" />
                                </svg>
                            </div>
                            <div>
                                <p class="text-sm font-semibold uppercase tracking-wide text-slate-400">
                                    Ranking global
                                </p>
                                <p class="text-xl font-bold text-slate-900">Desempenho dos melhores jogadores</p>
                            </div>
                        </div>
                    </header>

                    <div class="overflow-x-auto">
                        <table class="w-full table-auto text-left text-sm text-slate-600">
                            <thead class="text-xs uppercase tracking-wider text-slate-400">
                                <tr>
                                    <th class="pb-3 font-semibold">Posição</th>
                                    <th class="pb-3 font-semibold">Jogador</th>
                                    <th class="pb-3 font-semibold">Pontuação</th>
                                    <th class="pb-3 font-semibold">Tempo</th>
                                    <th class="pb-3 font-semibold">Precisão</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-100 text-base text-slate-800">
                                <tr v-for="player in props.leaderboard" :key="player.rank"
                                    class="hover:bg-emerald-50/30">
                                    <td class="py-4 font-semibold text-emerald-600">#{{ player.rank }}</td>
                                    <td class="py-4">{{ player.player }}</td>
                                    <td class="py-4 font-semibold">{{ player.score.toLocaleString('pt-BR') }}</td>
                                    <td class="py-4 text-slate-500">{{ player.time }}</td>
                                    <td class="py-4 text-slate-500">{{ player.accuracy }}%</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </section>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
