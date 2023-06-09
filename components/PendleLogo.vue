<script setup lang="ts">
import { computed } from 'vue';

const { width, height } = defineProps<{
    width?: number;
    height?: number;
}>();

const w = computed(() => width ?? (height ?? 200) / 2);
const h = computed(() => height ?? (width ?? 100) * 2);
const fillColor = '#063970';

</script>

<template>
    <svg :width="w" :height="h" viewBox="0 0 100 200">
        <circle r="50" cx="50" cy="150" :fill="fillColor" />
        <rect width="10" height="150" x="45" :fill="fillColor" />
    </svg>
</template>
