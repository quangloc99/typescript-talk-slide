<script setup lang="ts">
import { computed } from 'vue';

const props  = defineProps<{
    words?: string[];
    rowCount: number;
    colCount: number;
    offsetX?: number;
    offsetY?: number;
    spacing: number;
    cellClass?: string;
    maxRotation?: number;
    noise?: number;
}>();

type Cell = {
    x: number;
    y: number;
    text: string;
    rotate: number;
};

const sqrt3 = Math.sqrt(3);

const cells = computed(() => {
    const offsetX = props.offsetX ?? 0;
    const offsetY = props.offsetY ?? 0;
    const spacing = props.spacing ?? 10;
    const words = props.words ?? [];
    const noise = props.noise ?? 70;
    if (words.length == 0) return [];
    const maxRotation = props.maxRotation ?? Math.PI * 2;

    // https://en.wikipedia.org/wiki/Hexagonal_Efficient_Coordinate_System
    // Here I combined r and a into just r.
    // a can then be obtained by r & 1

    const res: Cell[] = [];

    for (let r = 0; r < props.rowCount; ++r) {
        for (let c = 0; c < props.colCount; ++c) {
            let x = (r & 1) / 2 + c;
            let y = sqrt3 * ((r & 1) / 2 + (r >> 1));
            x = x * spacing + (offsetX ?? 0) + (Math.random() * noise);
            y = y * spacing + (offsetY ?? 0) + (Math.random() * noise);
            const text = words[Math.floor(Math.random() * words.length)];
            const rotate = Math.random() * maxRotation;
            res.push({ x, y, text, rotate });
        }
    }
    return res;
});
</script>

<template>
    <div
        :class="{
            cell: true,
            [cellClass ?? '']: true,
        }"
        v-for="cell in cells"
        :style="{
            left: `${cell.x}px`,
            top: `${cell.y}px`,
            transform: `rotate(${cell.rotate}rad)`
        }"
    >
        {{ cell.text }}
    </div>
</template>

<style scoped>
.cell {
    position: absolute;
    z-index: -3;
}
</style>
