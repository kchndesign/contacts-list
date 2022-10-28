<!-- 
    This is a reusable component the displays a contact
    It's visibility is designed to be controlled by it's parent.
    Hook into the 'close' event to handle clicks that intend to close the modal 
-->

<script setup lang="ts">
import type { Contact } from "@/models/api";
import { defineEmits } from "vue";
import RemoveIcon from "../assets/remove-icon.svg?url";
import PersonInfoIcon from "../assets/personal-info-icon.svg?url";
import AddressLink from "./AddressLink.vue";
import CompanyHover from "./CompanyHover.vue";

// define props

defineProps<{
    isModalOpen: boolean;
    contact: Contact;
}>();

// define emits to close modal

const emit = defineEmits(["close"]);

// closeModal function emits close event

const closeModal = () => {
    emit("close");
};
</script>

<template>
    <!-- -------------------------- -->
    <!-- Conditionally render modal -->
    <!-- -------------------------- -->

    <div v-if="isModalOpen" class="modal__backdrop" @click="closeModal()">
        <div class="modal" @click.stop>
            <div class="modal__header">
                <img
                    :src="PersonInfoIcon"
                    class="modal__icon"
                    alt="drawing of person and information icon"
                />

                <h2 class="modal__title">{{ contact.name }}</h2>

                <button class="modal__close-button" @click="closeModal()">
                    <img :src="RemoveIcon" alt="close modal" width="16px" />
                </button>
            </div>

            <div class="modal__list">
                <hr />
                <p>Phone: {{ contact.phone }}</p>
                <hr />
                <p>Email: {{ contact.email }}</p>
                <hr />
                <p>Website: {{ contact.website }}</p>
                <hr />
                <p>Twitter: @{{ contact.username }}</p>
                <hr />
                <p>Address: <AddressLink :address="contact.address" /></p>
                <hr />
                <p>Company: <CompanyHover :company="contact.company" /></p>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
/* ------------- */
/* Modal Styling */
/* ------------- */

.modal {
    /* modal stuff */
    position: fixed;

    top: 50%;
    left: 50%;

    transform: translate(-50%, -50%);

    overflow-y: scroll;

    /* style stuff */

    background-color: white;
    width: min(55rem, calc(100% - 2rem));
    max-height: calc(100% - 2rem);

    padding: 2rem;
    padding-bottom: 4rem; // for the close button on mobile layouts
    border-radius: 5px;

    box-shadow: 2px 2px 15px #19191919;
}

.modal__backdrop {
    position: fixed;

    top: 0;
    left: 0;

    width: 100%;
    height: 100%;

    background-color: #19191919;
}

/* --------------------- */
/* Modal Content styling */
/* --------------------- */

.modal {
    /* --- Modal Header --- */

    &__header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        flex-direction: column;

        gap: 1.5rem;

        margin-bottom: 1rem;

        @media (min-width: 768px) {
            flex-direction: row;
        }
    }

    /* --- Modal Title --- */

    &__title {
        flex-basis: 100%;
        flex-grow: 1;
        align-self: flex-start;

        @media (min-width: 768px) {
            align-self: unset;
        }
    }

    /* --- Modal Icon --- */

    &__icon {
        min-width: 5rem;
        max-width: 50%;
        flex-basis: 5rem;
        flex-grow: 1;
    }

    /* --- Modal List --- */
    &__list {
        display: flex;
        flex-direction: column;
        row-gap: 0.75rem;

        hr {
            width: 100%;
            color: #19191919;
        }
    }
}

/* -------------------- */
/* Modal Button styling */
/* -------------------- */

.modal__close-button {
    border: none;
    background-color: transparent;
    cursor: pointer;
    padding: 1rem;

    &:hover {
        background-color: #19191919;
    }

    img {
        width: 2rem;
    }

    // button is fixed and bigger on small screens
    // i put the button on the bottom right to make it easier for right handed thumb users

    position: fixed;
    bottom: 0rem;
    right: 0rem;

    @media (min-width: 768px) {
        position: relative;
    }
}
</style>
