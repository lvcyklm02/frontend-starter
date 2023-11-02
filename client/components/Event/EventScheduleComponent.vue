<script setup lang="ts">
import CreateEventForm from "@/components/Event/CreateEventForm.vue";
import EventBlockComponent from "@/components/Event/EventBlockComponent.vue";
import { useUserStore } from "@/stores/user";
import { fetchy } from "@/utils/fetchy";
import { storeToRefs } from "pinia";
import { onBeforeMount, ref } from "vue";

const { currentUsername, isLoggedIn } = storeToRefs(useUserStore());

const loaded = ref(false);
let events = ref<Array<Record<string, string>>>([]);
let adding = ref(false);

async function getEvents() {
    const username = currentUsername.value
    let query: Record<string, string> = { username };
    let eventResults;
    try {
        eventResults = await fetchy("api/events", "GET", { query });
    } catch (_) {
        return;
    }
    events.value = eventResults;
}

function updateAdding() {
    adding.value = !adding.value;
}

onBeforeMount(async () => {
    await getEvents();
    loaded.value = true;
});
</script>

<template>
    <main v-if="!adding" class="eventSchedule">
        <div class="row">
            <h2>Events:</h2>
        </div>
        <section v-if="loaded && events.length !== 0">
            <article v-for="event in events" :key="event._id">
                <EventBlockComponent :event="event" />
            </article>
        </section>
        <p v-else-if="loaded">No events found</p>
        <p v-else>Loading...</p>
        <button @click="updateAdding">Add Event</button>
    </main>
    <CreateEventForm v-else @refreshEvents="getEvents" @addEvent="updateAdding" />
</template>

<style scoped>
section {
    display: flex;
    flex-direction: column;
    gap: 1em;
}

section,
p,
.row {
    margin: 0 auto;
    max-width: 60em;
}

main {
    background-color: var(--base-bg);
    border-radius: 1em;
    display: flex;
    flex-direction: column;
    gap: 0.5em;
    padding: 1em;
    max-width: 60em;
    justify-content: center;
}

.posts {
    padding: 1em;
}

.row {
    display: flex;
    justify-content: space-between;
    margin: 0 auto;
    max-width: 60em;
}
</style>
