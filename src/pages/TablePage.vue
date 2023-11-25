<template>
  <q-page padding>
    <p class="text-h6">Table</p>
    <q-table
      flat
      bordered
      title="Treats"
      :rows="rows"
      :columns="columns"
      row-key="name"
      :selected-rows-label="getSelectedString"
      selection="multiple"
      v-model:selected="selected"
    />
  </q-page>
</template>

<script setup lang="ts">
import { QTableProps, useQuasar } from 'quasar';
import { api } from 'src/boot/axios';
import { onMounted, ref } from 'vue';

const $q = useQuasar();

const selected = ref([]);
const rows = ref([]);

function getSelectedString() {
  return selected.value.length === 0
    ? ''
    : `${selected.value.length} record${
        selected.value.length > 1 ? 's' : ''
      } selected of ${rows.value.length}`;
}

const columns: QTableProps['columns'] = [
  {
    name: 'desc',
    required: true,
    label: 'Dessert (100g serving)',
    align: 'left',
    field: (row: { name: string }) => row.name,
    format: (val: unknown) => `${val}`,
    sortable: true,
  },
  {
    name: 'calories',
    align: 'center',
    label: 'Calories',
    field: 'calories',
    sortable: true,
  },
  { name: 'fat', label: 'Fat (g)', field: 'fat', sortable: true },
  { name: 'carbs', label: 'Carbs (g)', field: 'carbs' },
  { name: 'protein', label: 'Protein (g)', field: 'protein' },
  { name: 'sodium', label: 'Sodium (mg)', field: 'sodium' },
  {
    name: 'calcium',
    label: 'Calcium (%)',
    field: 'calcium',
    sortable: true,
    sort: (a: string, b: string) => parseInt(a, 10) - parseInt(b, 10),
  },
  {
    name: 'iron',
    label: 'Iron (%)',
    field: 'iron',
    sortable: true,
    sort: (a: string, b: string) => parseInt(a, 10) - parseInt(b, 10),
  },
];

async function loadTable() {
  $q.loading.show();
  const res = await api.get('table');
  setTimeout(() => {
    rows.value = res.data.rows;
    $q.loading.hide();
  }, 1000);
}
onMounted(() => {
  loadTable();
});
</script>
