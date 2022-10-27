<!-- 
    This is a reusable component that displays a company name
    but allows the user to hover over the text to reveal more information
    about the company.
-->

<script setup lang="ts">
import type { Company } from "@/models/api";

// define props
defineProps<{
    company: Company;
}>();
</script>

<template>
    <span class="company-hover">
        {{ company.name }}
        <div class="tooltip">
            <p>{{ company.catchPhrase }}</p>
            <p>{{ company.bs }}</p>
        </div>
    </span>
</template>

<style lang="scss" scoped>
// tooltip design taken from Louis Hoebregts
// ref: https://css-tricks.com/perfect-tooltips-with-css-clipping-and-masking/
.tooltip {
    clip-path: polygon(
        0% 0%,
        // Top left point
        100% 0%,
        // Top right point
        100% calc(100% - 10px),
        // Bottom right point
        calc(50% + 10px) calc(100% - 10px),
        // Center right of the triangle
        50% 100%,
        // Tip of the triangle
        calc(50% - 10px) calc(100% - 10px),
        // Center left of the triangle
        0% calc(100% - 10px) // Bottom left point
    );

    // default state is hidden
    visibility: hidden;

    // position above and in the center
    position: absolute;
    top: 0;
    left: 50%;
    transform: translate(-50%, -100%);

    // style and size
    background-color: #f7f7f8;
    width: max-content;
    max-width: 20rem;
    padding: 1rem;
    padding-bottom: 1.25rem;
}

.company-hover {
    position: relative;

    &:hover .tooltip {
        visibility: visible;
    }
}
</style>
