<!-- 
    A reusable component that takes an Address entity, 
    formats the address into a single string,
    and allows the user to navigate to a google maps link
-->

<script setup lang="ts">
import type { Address, LocationLiteral } from "@/models/api";

// declare props
defineProps<{
    address: Address;
}>();

// Utility function to format the address as a single string

const formatAddressAsSentence = (a: Address): string => {
    return `${a.suite} ${a.street}, ${a.city} ${a.zipcode}`;
};

// utility function to generate google maps search address

const generateGoogleMapsUrlFromLocationLiteral = (
    loc: LocationLiteral
): string => {
    // return "https://google.com";

    return (
        "https://www.google.com/maps/search/?api=1&" +
        encodeURIComponent(loc.lat + "," + loc.lng)
    );
};
</script>

<template>
    <a
        target="_blank"
        :href="generateGoogleMapsUrlFromLocationLiteral(address.geo)"
        >{{ formatAddressAsSentence(address) }}
    </a>
</template>

<style lang="scss" scoped></style>
