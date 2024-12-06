<script setup lang="ts">
import { ref, computed } from 'vue';
import { useElementSize } from '@vueuse/core';

const count = ref(30);
const columnWidth = 150;
const columnPadding = 12;

const el = ref<HTMLElement | null>(null);
const colIndex = ref<number | null>(null);

const { width, height } = useElementSize(el);

const columns = computed(() => {
  return Math.max(1, Math.floor((width.value || 0) / (columnWidth + columnPadding * 2)));
});

const rowIndex = computed(() => {
  return Math.floor((colIndex.value ?? 0) / columns.value);
});

const lastIndexInRow = computed(() => {
  return (rowIndex.value + 1) * columns.value - 1;
});
</script>

<template>
  <div ref="el">
    <div>Columns: {{ columns }}</div>
    <div>Width: {{ width }}, Height: {{ height }}</div>
    <VRow>
      <template v-for="(n, index) in count" :key="index">
        <v-col cols="auto">
          <v-card width="150" :title="index">
            <v-card-actions>
              <v-icon
                :icon="colIndex === index ? 'mdi-chevron-down' : 'mdi-chevron-up'"
                @click="colIndex = colIndex === index ? null : index"
              />
            </v-card-actions>
          </v-card>
        </v-col>
        <!-- Expandable Content -->
        <v-col v-show="index === lastIndexInRow  && colIndex !== null" cols="12">
          <div>
            Expandable content for index {{ colIndex }}. Add any details here.
          </div>
        </v-col>
      </template>
    </VRow>
  </div>
</template>
