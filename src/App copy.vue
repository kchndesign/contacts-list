<template>
    <main class="content-wrapper">
        <h1 class="font-light">Contacts</h1>

        <template v-for="user in contacts" :key="user.id">
            <p>{{ user.name }}</p>
        </template>
    </main>
</template>

<script setup lang="ts">
import { onMounted, reactive } from "vue";
import axios from "axios";
import type { Contact } from "./models/api";

// define reactive contacts list

let contacts: Array<Contact> = reactive([]);

// init function

const init = async () => {
    const response: { data: Contact[] } = await axios.get(
        "https://jsonplaceholder.typicode.com/users"
    );

    contacts = response.data;
};

// onmount hook
onMounted(() => {
    init();
});
</script>

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
</style>
