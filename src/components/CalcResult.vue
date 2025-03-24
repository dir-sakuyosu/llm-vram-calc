<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  params: number | null
  precision: number | null
  contextSize: number | null
}>()

const requiredSize = computed(() => {
  if (props.params === null || props.precision === null || props.contextSize === null) {
    return undefined
  }
  const sz =
    ((props.params * 1000000000 * props.precision) / 8 + 1.29 * 100000 * 4.06 * props.contextSize) /
    (1024 * 1024 * 1024)
  return Math.round(sz)
})
</script>

<template>
  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">必要VRAM</label>
    </div>
    <div class="field-body">
      <div class="field has-addons">
        <div class="control">
          <label class="label field-text">{{ requiredSize }}</label>
        </div>
        <div class="control">
          <label class="label field-text">GB</label>
        </div>
      </div>
    </div>
  </div>
</template>
