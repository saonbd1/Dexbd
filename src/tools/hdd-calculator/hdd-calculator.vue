<script setup lang="ts">
import InputCopyable from '../../components/InputCopyable.vue';

const units = [
  { value: 'kb', label: 'KB' },
  { value: 'mb', label: 'MB' },
  { value: 'gb', label: 'GB' },
  { value: 'tb', label: 'TB' },
];

const claimedCapacity = ref(1);
const claimedUnit = ref('tb');

const unitsConversion = {
  kb: { dec: 1000, bin: 1024 },
  mb: { dec: 1000000, bin: 1024 * 1024 },
  gb: { dec: 1000000000, bin: 1024 * 1024 * 1024 },
  tb: { dec: 1000000000000, bin: 1024 * 1024 * 1024 * 1024 },
};

type Units = 'kb' | 'mb' | 'gb' | 'tb';
function getRealSize(toUnit: Units) {
  const fromUnit = unitsConversion[claimedUnit.value as Units];
  const toUnitBin = unitsConversion[toUnit as Units].bin;
  return claimedCapacity.value * fromUnit.dec / toUnitBin;
};
</script>

<template>
  <div>
    <n-form-item label="Claimed Capacity:">
      <n-input-number v-model:value="claimedCapacity" :min="1" />
    </n-form-item>
    <c-select
      v-model:value="claimedUnit"
      label="Unit:"
      :options="units"
    />

    <n-divider />

    <InputCopyable
      v-for="({ value, label }) in units"
      :key="value"
      :label="`Capacity in ${label}`"
      :value="getRealSize(value as Units).toFixed(5)"
    />
  </div>
</template>
