<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";
import { onBeforeMount, ref } from "vue";
import { fetchy } from "../../utils/fetchy";

const props = defineProps(["event"]);
const emit = defineEmits(["editPost", "refreshEvents"]);
const { currentUsername } = storeToRefs(useUserStore());
const isRegistered = ref(false);
const loaded = ref(false);

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

const getIsRegistered = async () => {
    try {
        isRegistered.value = await fetchy(`api/events/register/${props.event._id}`, "GET");
    } catch {
        return;
    }
};

const registerForEvent = async () => {
    try {
        await fetchy(`api/events/register/${props.event._id}`, "PATCH");
    } catch {
        return;
    }
};

const unRegisterForEvent = async () => {
    try {
        await fetchy(`api/events/unregister/${props.event._id}`, "PATCH");
    } catch {
        return;
    }
};

onBeforeMount(async () => {
    await getIsRegistered();
    loaded.value = true;
});
</script>

<template>
    <p class="start">{{ props.event.start }}</p>
    <p class="end">{{ props.event.end }}</p>
    <p class="organizer">{{ props.event.organizer }}</p>
    <p class="capacity">{{ registeredCount }}/{{ props.event.capacity }}</p>
    <p class="roster">{{ props.event.roster }}</p>
    <button v-if="!isRegistered" @click="registerForEvent">Register</button>
    <button v-else @click="unRegisterForEvent">Cancel registration</button>
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
