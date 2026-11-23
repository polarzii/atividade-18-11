
<script setup>
import { computed } from 'vue';
import { entregas } from '../entregas.js';
import {
    ordenarPorData,
    filtrarPendentes
} from '../agenda.js';

import CartaoTipo from '../components/CartaoTipo.vue';

const TIPOS = [
    { tipo: 'prova', titulo: 'Provas' },
    { tipo: 'trabalho', titulo: 'Trabalhos' },
    { tipo: 'apresentacao', titulo: 'Apresentações' }
];

const resumos = computed(() => {
    return TIPOS.map((item) => {
        const entregasDoTipo = entregas.value.filter(
            entrega => entrega.tipo === item.tipo
        );

        const feitas = entregasDoTipo.filter(
            entrega => entrega.feita
        ).length;

        const pendentes = ordenarPorData(
            filtrarPendentes(entregasDoTipo)
        );

        return {
            tipo: item.tipo,
            titulo: item.titulo,
            total: entregasDoTipo.length,
            feitas: feitas,
            pendentes: pendentes
        };
    });
});
</script>

<template>
    <main class="mx-auto max-w-5xl p-5">
        <h2 class="mb-6 text-2xl font-bold">
            Entregas por tipo
        </h2>

        <div class="grid gap-6 md:grid-cols-3">
            <CartaoTipo
                v-for="resumo in resumos"
                :key="resumo.tipo"
                :resumo="resumo"
            />
        </div>
    </main>
</template>
