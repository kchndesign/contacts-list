<script setup lang="ts">
import { onMounted, ref } from "vue";
import axios from "axios";
import type { Contact } from "./models/api";
import ContactCard from "./components/ContactCard.vue";

/*=============================================
=            Contacts list fetch and sort            =
=============================================*/

// define a FilteredContactsArray that stores the character it is filtering by and the array of results

type FilteredContactsArray = {
    filteredCharacter: string;
    contacts: Array<Contact>;
};

let filteredContactsArrays = ref<Array<FilteredContactsArray>>([]);

/**
 * Generate a list of FileredContactArray entities from an unfiltered list.
 * @param contacts master array of unfiltered contacts
 */
const generateFilteredContactsArraysFromMasterList = (
    contacts: Array<Contact>
): Array<FilteredContactsArray> => {
    //
    // generate a list of first letters in contact.name
    const lettersSet = new Set(contacts.map((contact) => contact.name[0]));

    // init array of FilteredContactsArray
    const resultingArray: Array<FilteredContactsArray> = [];

    // function to sort by alphabet
    const compareStrings = (a: string, b: string): number => {
        if (a < b) return -1;
        if (a > b) return 1;
        return 0;
    };

    // for each value in the set, make a new FilteredContactsArray
    lettersSet.forEach((letter) => {
        const filteredContactsArray: FilteredContactsArray = {
            filteredCharacter: letter,
            contacts: contacts
                .filter((contact) => contact.name[0] == letter)
                .sort((a, b) => compareStrings(a.name, b.name)),
        };

        resultingArray.push(filteredContactsArray);
    });

    // sort the resulting array by the filtered letter
    resultingArray.sort((a, b) =>
        compareStrings(a.filteredCharacter, b.filteredCharacter)
    );

    return resultingArray;
};

// init function

const init = async () => {
    const response = await axios.get(
        "https://jsonplaceholder.typicode.com/users"
    );

    filteredContactsArrays.value = generateFilteredContactsArraysFromMasterList(
        response.data
    );
};

// onmount hook
onMounted(() => {
    init();
});

/*=====  End of Contacts list fetch and sort  ======*/
</script>

<template>
    <main class="content-wrapper">
        <h1 class="font-light">Contacts</h1>

        <div class="contacts-list">
            <template
                v-for="(filteredContactsArray, index) in filteredContactsArrays"
                :key="index"
            >
                <h2>{{ filteredContactsArray.filteredCharacter }}</h2>
                <ContactCard
                    v-for="contact in filteredContactsArray.contacts"
                    :contact="contact"
                    :key="contact.id"
                />
            </template>
        </div>
    </main>
</template>

<style scoped>
/* Utility classes */

.font-light {
    font-weight: 300;
}

/* Layout and content classes */
.content-wrapper {
    max-width: 57rem;
    padding: 2rem;
    margin-inline: auto;
    min-height: 100vh;
    background-color: #f7f7f8;
    /* box-shadow: 5px 5px 15px #19191919; */
}

.contacts-list {
    margin-top: 2rem;

    display: flex;
    flex-direction: column;
    row-gap: 1rem;
}
</style>
