<template>
  <q-page padding>
    <p class="text-h6">Dashboard</p>
    <div class="q-pa-md">
      <div class="row">
        <div
          class="col q-pa-md"
          v-for="card in state.cardRecords"
          :key="card.title"
        >
          <q-card
            class="my-card text-white"
            style="
              background: radial-gradient(circle, #35a2ff 0%, #014a88 100%);
            "
          >
            <q-card-section>
              <div class="text-h6">{{ card.title }}</div>
              <div class="text-subtitle2">
                {{ card.content }}
              </div>
            </q-card-section>
          </q-card>
        </div>
      </div>
    </div>

    <div v-if="state.options.dataset.source.length">
      <q-card class="no-shadow" bordered>
        <q-card-section class="text-h6"> Line Chart </q-card-section>
        <q-card-section>
          <ECharts
            :option="state.options"
            class="q-mt-md"
            :resizable="true"
            autoresize
            style="height: 300px"
          />
        </q-card-section>
      </q-card>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import ECharts from 'vue-echarts';
import 'echarts';
import { onMounted, reactive } from 'vue';
import { useQuasar } from 'quasar';
import { api } from 'src/boot/axios';

const $q = useQuasar();

type CardContent = {
  title: string;
  content: string;
};

const state = reactive({
  cardRecords: [] as CardContent[],
  options: {
    legend: {
      bottom: 10,
    },
    tooltip: {},
    dataset: {
      source: [],
    },
    grid: {
      left: '3%',
      right: '4%',
      bottom: '20%',
      top: '5%',
      containLabel: true,
    },
    xAxis: { type: 'category' },
    yAxis: {},
    // Declare several bar series, each will be mapped
    // to a column of dataset.source by default.
    series: [{ type: 'bar' }, { type: 'bar' }, { type: 'bar' }],
  },
});

async function loadDashboard() {
  $q.loading.show();
  const res = await api.get('dashboard');
  setTimeout(() => {
    state.cardRecords = res.data.cards;
    state.options.dataset.source = res.data.graph;
    $q.loading.hide();
  }, 1000);
}
onMounted(() => {
  loadDashboard();
});
</script>

<style scoped lang="scss"></style>
