<script setup lang="ts">
import PendleLogo from '../components/PendleLogo.vue';
</script>

<template>
    <div class="slidev-layout section pendle-section">
        <div class="background">
            <PendleLogo class="pendle-logo" :width="900" />
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
    transform: translate(100%, 0);
}

.pendle-section-enter-from .slot,
.pendle-section-leave-to .slot {
    opacity: 0;
}

/* .pendle-logo,
.slot {
    transition: all 1.5s ease;
} */

.background {
    opacity: 0.6;
    z-index: -1;
}

.pendle-logo {
    position: absolute;
    right: 0;
    transform: translate(50%, 0);
    top: -850px;
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
