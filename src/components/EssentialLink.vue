<template>
  <q-item clickable :to="path" exact>
    <q-item-section v-if="icon" avatar>
      <q-icon :name="icon" />
    </q-item-section>

    <q-item-section>
      <q-item-label>{{ title }}</q-item-label>
    </q-item-section>
  </q-item>
</template>

<script setup lang="ts">
export interface EssentialLinkProps {
  title: string;
  path?: string;
  icon?: string;
}

withDefaults(defineProps<EssentialLinkProps>(), {
  path: '#',
  icon: '',
});
</script>
