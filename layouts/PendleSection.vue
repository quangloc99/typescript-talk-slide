<script setup lang="ts">
import PendleLogo from '../components/PendleLogo.vue';
import RandomWordsHexaGrid from '../components/RandomWordsHexaGrid.vue';

const props = defineProps<{
    backgroundWords?: string[];
}>();
</script>

<template>
    <div class="slidev-layout section pendle-section">
        <div class="background">
            <PendleLogo class="pendle-logo" :width="900" />
            <RandomWordsHexaGrid
                :row-count="5"
                :col-count="7"
                :spacing="200"
                :words=props.backgroundWords
                :max-rotation="Math.PI / 2"
                cell-class="cell"
            />
        </div>
        <div h-full grid>
            <div my-auto class="slot">
                <slot />
            </div>
        </div>
    </div>
</template>

<style>
.pendle-section-enter-from .pendle-logo,
.pendle-section-leave-to .pendle-logo {
    transform: translate(55%, 100%);
}

.pendle-section-enter-from .cell,
.pendle-section-leave-to .cell {
    left: calc(150%) !important;
    transform: rotate(0) !important;
}

.pendle-section-enter-from .slot,
.pendle-section-leave-to .slot {
    opacity: 0;
}

.background {
    opacity: 0.3;
    z-index: -1;
}

.pendle-logo {
    position: absolute;
    right: 0;
    transform: translate(50%, 0);
    top: -850px;
}

.cell {
    font-size: 1em;
    color: gray;
}

.slot {
    z-index: 1;
    opacity: 1;
}

/* @keyframes PendleLogoShowUp {
    from {
        transform: translate()
    }
} */
</style>
