<!-- 
    Display a contact as a card 
    I've also made this card component manage its own modal 
    This is because the card component directly knows when it is clicked, 
    and what contact data to display in the modal. 
    It also can encapsulate and mange its own modalIsOpen boolean. 
    Which avoids a parent having to manage the modalIsOpen states for many different contacts.
 -->

<script setup lang="ts">
import type { Contact } from "@/models/api";
import { ref } from "vue";
import ContactModal from "./ContactModal.vue";

// define props

defineProps<{
    contact: Contact;
}>();

// setup modal state and event handlers

let isModalOpen = ref<boolean>(false);

// function to open modal is used by the card click
const openModal = () => {
    isModalOpen.value = true;
};

// function to close modal is used by a number of elements

const closeModal = () => {
    isModalOpen.value = false;
};
</script>

<template>
    <div class="card" @click="openModal()">
        <h3 class="font-semibold">{{ contact.name }}</h3>
        <p class="desktop-only asdf">Phone: {{ contact.phone }}</p>
    </div>

    <!-- conditionally render modal -->

    <ContactModal
        :is-modal-open="isModalOpen"
        :contact="contact"
        @close="closeModal()"
    />
</template>

<style lang="scss" scoped>
/* Utility classes */

.font-semibold {
    font-weight: 600;
}

.desktop-only {
    display: none;

    @media (min-width: 768px) {
        display: block;
    }
}

/* ------------ */
/* Card styling */
/* ------------ */

.card {
    padding: 1rem;
    background-color: white;

    border-radius: 5px;
    cursor: pointer;

    &:hover {
        outline: 3px solid lightblue;
    }
}
</style>
