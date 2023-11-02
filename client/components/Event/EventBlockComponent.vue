<script setup lang="ts">
import EventDetailComponent from "@/components/Event/EventDetailComponent.vue";
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";
import { fetchy } from "../../utils/fetchy";

const props = defineProps(["event"]);
const emit = defineEmits(["editPost", "refreshEvents"]);
const { currentUsername } = storeToRefs(useUserStore());

const registeredCount = props.event.roster.length

const deleteEvent = async () => {
    try {
        await fetchy(`api/events/${props.event._id}`, "DELETE");
    } catch {
        return;
    }
    emit("refreshEvents");
};

const sendRefreshEvents = async () => {
    emit("refreshEvents");
};
</script>

<template>
    <p class="start">{{ props.event.start }}</p>
    <p class="end">{{ props.event.end }}</p>
    <p class="organizer">{{ props.event.organizer }}</p>
    <p class="capacity">{{ registeredCount }}/{{ props.event.capacity }}</p>

    <EventDetailComponent :event="event" />
</template>

<style scoped>
p {
    margin: 0em;
}

.author {
    font-weight: bold;
    font-size: 1.2em;
}

menu {
    list-style-type: none;
    display: flex;
    flex-direction: row;
    gap: 1em;
    padding: 0;
    margin: 0;
}

.timestamp {
    display: flex;
    justify-content: flex-end;
    font-size: 0.9em;
    font-style: italic;
}

.base {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.base article:only-child {
    margin-left: auto;
}
</style>
