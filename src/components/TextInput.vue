<script lang="ts" setup>
import Textarea from 'primevue/textarea';
const modelValue = defineModel<string>({ required: true })
</script>

<template>
    <div style="display: flex; flex-direction: column;">
        <Textarea v-model="modelValue" style="flex: 1;"></Textarea>
        <div>
            {{ modelValue.length }} chars | {{ modelValue.split(/\n/).length }} lines
        </div>
    </div>
</template>