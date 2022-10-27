<!-- Display a contact as a card -->

<script setup lang="ts">
import type { Contact } from "@/models/api";
import { defineProps } from "vue";

// define props

defineProps<{
    contact: Contact;
}>();

//  define emits
defineEmits(["click"]);
</script>

<template>
    <div class="card" @click="$emit('click')">
        <h3 class="font-semibold">{{ contact.name }}</h3>
        <p class="desktop-only asdf">Phone: {{ contact.phone }}</p>
    </div>
</template>

<style lang="css" scoped>
/* Utility classes */

.font-semibold {
    font-weight: 600;
}

.desktop-only {
    display: none;
}

@media screen and (min-width: 768px) {
    .desktop-only {
        display: block;
    }
}

/* Styling */
.card {
    padding: 1rem;
    background-color: white;

    border-radius: 5px;
    cursor: pointer;
}

.card:hover {
    outline: 3px solid lightblue;
}
</style>
