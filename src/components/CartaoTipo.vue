
<script setup>
import { computed } from 'vue';
import { formatarData } from '../agenda.js';

const props = defineProps({
    resumo: {
        type: Object,
        required: true
    }
});

function escolherCorDoTipo(tipo) {
    if (tipo === 'prova') {
        return 'bg-red-600';
    }

    if (tipo === 'trabalho') {
        return 'bg-blue-600';
    }

    return 'bg-purple-600';
}

function calcularPorcentagemFeita() {
    if (props.resumo.total === 0) {
        return 0;
    }

    return Math.round(
        (props.resumo.feitas / props.resumo.total) * 100
    );
}

const cor = computed(() => escolherCorDoTipo(props.resumo.tipo));

const porcentagem = computed(() => calcularPorcentagemFeita());

const classeBorda = computed(() => {
    if (props.resumo.pendentes.length === 0) {
        return 'border-emerald-400';
    }

    return 'border-gray-200';
});
</script>

<template>
    <section
        :class="[
            classeBorda,
            'rounded-xl border bg-white p-6 shadow-sm transition hover:-translate-y-1 hover:shadow-md'
        ]"
    >
        <div class="flex items-center justify-between gap-2">
            <h3 class="m-0 text-lg font-bold">
                {{ resumo.titulo }}
            </h3>

            <span
                :class="[
                    cor,
                    'rounded-full px-3 py-1 text-sm font-bold text-white'
                ]"
            >
                Pendentes: {{ resumo.pendentes.length }}
            </span>
        </div>

        <p class="mb-2 mt-4 text-sm text-gray-600">
            {{ resumo.feitas }} de {{ resumo.total }} feitas
        </p>

        <div class="h-2 overflow-hidden rounded-full bg-gray-200">
            <div
                :class="[cor, 'h-full rounded-full transition-all']"
                :style="{ width: porcentagem + '%' }"
            ></div>
        </div>

        <ul class="mt-5 list-none space-y-2 p-0">
            <li
                v-for="entrega in resumo.pendentes"
                :key="entrega.id"
                class="flex items-center justify-between gap-2 rounded-lg bg-gray-50 p-3"
            >
                <RouterLink
                    :to="`/entrega/${entrega.id}`"
                    class="font-bold text-gray-800 hover:underline"
                >
                    {{ entrega.materia }}
                </RouterLink>

                <span class="shrink-0 text-sm text-gray-500">
                    {{ formatarData(entrega.data) }}
                </span>
            </li>
        </ul>

        <p
            v-if="resumo.pendentes.length === 0"
            class="mb-0 mt-4 text-sm text-gray-500"
        >
            Nada pendente.
        </p>
    </section>
</template>
