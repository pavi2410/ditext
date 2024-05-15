<script setup lang="ts">
import { computed, ref } from 'vue'
import { diffChars, diffLines, diffWords } from 'diff'
import TextInput from './components/TextInput.vue'
import SelectButton from 'primevue/selectbutton'
import Checkbox from 'primevue/checkbox'

const textA = ref('detect\ndiff text')
const textB = ref('ditext\nditext')

const strategy = ref('char')
const ignoreCase = ref(false)

const diff = computed(() => {
  const options = {
    ignoreCase: ignoreCase.value
  }

  if (strategy.value === 'line') {
    return diffLines(textA.value, textB.value, options)
  }
  if (strategy.value === 'word') {
    return diffWords(textA.value, textB.value, options)
  }
  return diffChars(textA.value, textB.value, options)
})
</script>

<template>
  <div style="height: 100vh; padding: 8px; display: flex; flex-direction: column;">
    <div style="display: flex; flex-direction: row;">
      <SelectButton v-model="strategy" :options="['char','word','line']" style="margin-bottom: 8px"></SelectButton>
      <div>
        <Checkbox v-model="ignoreCase" binary input-id="ignoreCase"></Checkbox>
        <label for="ignoreCase">Ignore Case</label>
      </div>
    </div>
    <div style="display: flex; flex-direction: row; gap: 8px; flex: 1;">
      <TextInput v-model="textA" style="height: 100%; flex: 1"></TextInput>
      <TextInput v-model="textB" style="height: 100%; flex: 1"></TextInput>
      <div style="height: 100%; flex: 1; white-space: pre; overflow-x: scroll; border: 1px solid #eee3; border-radius: 8px; padding: 8px;">
        <code>
          <span v-for="part in diff" :style="{ backgroundColor: part.added ? '#0f07' : part.removed ? '#f007' : 'transparent' }">
            {{ part.value }}
          </span>
        </code>
      </div>
    </div>
  </div>
</template>

<style>
html {
  font-family: 'Inter Variable', sans-serif;
}
body {
  margin: 0;
  padding: 0;
}
</style>
