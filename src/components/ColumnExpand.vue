<script setup lang="ts">
import { useElementSize } from '@vueuse/core'
import { ref, computed } from 'vue';
import { VRow } from 'vuetify/components'

const count = ref(30)
const columnWidht = 150
const columnPadding = 12
const el = ref(null)
const colIndex = ref<number | null>(null)
const rowRef = ref<HTMLElement | null>(null)
const rowCol = ref<HTMLElement | null>(null)
const { width, height } = useElementSize(rowRef)
const { width: colWidth, height: colHeight } = useElementSize(rowCol)

const columns = computed(() => {
  return Math.floor(width.value / (columnWidht + columnPadding * 2))
})

const rowIndex = computed(() => {
  return Math.floor(colIndex.value / 10)
})

const lastIndexInRow = computed(() => {
  return (rowIndex.value + 1) * columns.value - 1
})
</script>

<template>
  <div>{{ columns }}</div>
  <div ref="el">
    Height: {{ height }}
    Width: {{ width }}
  </div>

  <div ref="el">
    Height: {{ colHeight }}
    Width: {{ colWidth }}
  </div>
  <VRow
    ref="rowRef"
    justify="space-between"
  >
    <template
      v-for="(n, index) in count"
      :key="n"
    >
      <v-col
        ref="rowCol"
        cols="auto"
      >
        <v-card
          width="150"
          :title="index"
        >
          <v-card-actions>
            <v-icon
              :icon="colIndex !== index ? 'mdi-chevron-up' : 'mdi-chevron-down'"
              @click="colIndex = colIndex === null ? index : colIndex === index ? null : index"
            />
          </v-card-actions>
        </v-card>
      </v-col>

      <v-col
        v-if="index === lastIndexInRow && colIndex !== null"
        cols="12"
        width="1740"
      >
        <!-- <VRow
          class="border-sm"
          justify="space-between"
        >
          <v-col
            v-for="(n, childIndex) in columns"
            :key="n"
            cols="auto"
          >
            <v-card
              width="150"
            >
              <v-card-actions
                v-if="colIndex % columns === childIndex"
              >
                <v-btn icon="mdi-chevron-up" />
              </v-card-actions>
            </v-card>
          </v-col>
        </VRow> -->

        <v-toolbar
          density="compact"
          rounded
        >
          <VRow
            class="border-sm"
            justify="space-between"
          >
            <v-col
              v-for="(n, childIndex) in columns"
              :key="n"
              cols="auto"
            >
              <v-card
                width="150"
                color="transparent"
                flat
              >
                <v-card-actions>
                  <v-icon
                    v-if="colIndex % columns === childIndex"
                    icon="mdi-chevron-up"
                    variant="plain"
                    @click="colIndex = colIndex === null ? index : null"
                  />
                </v-card-actions>
              </v-card>
            </v-col>
          </VRow>
        </v-toolbar>
        <div>
          Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.
        </div>
      </v-col>
    </template>
  </VRow>
</template>

<style lang="scss" scoped>
.colWidth::deep() {
  width: 1740px !important ;
}
</style>
