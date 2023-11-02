<script setup lang="ts">
import { ref } from "vue";
import { fetchy } from "../../utils/fetchy";

const props = defineProps(["post"]);
const content = ref("");
const capacity = ref<number>();
const month = ref<number>();
const day = ref<number>();
const startHour = ref<number>();
const startMinute = ref<number>();
const endHour = ref<number>();
const endMinute = ref<number>();
const emit = defineEmits(["addEvent", "refreshEvents"]);

const createEvent = async (content: string, capacity?: number, month?: number, day?: number, startHour?: number, startMinute?: number, endHour?: number, endMinute?: number) => {
    const isNotUndefined = (inp: string | number | undefined) => {
        return (inp !== undefined);
    }

    const inputs = [content, capacity, month, day, startHour, startMinute, endHour, endMinute];
    if (inputs.every(isNotUndefined)) {
        try {
            const response = await fetchy("api/events", "POST", {
                body: { ...inputs },
            });

        } catch (_) {
            console.log("failed to create event");
            return;
        }
    }

    emit("refreshEvents");
    emit("addEvent");
    emptyForm();
};

const emptyForm = () => {
    content.value = "";
};
</script>

<template>
    <form @submit.prevent="createEvent(content, capacity, month, day, startHour, startMinute, endHour, endMinute)">
        <label for="content">New Event Details:</label>
        <textarea id="content" v-model="content" placeholder="Event Description" required> </textarea>
        <input id="capacity" v-model.number="capacity" type="number" placeholder="Capacity" required>
        <input id="month" v-model.number="month" type="number" placeholder="MM" required>
        <input id="day" v-model.number="day" type="number" placeholder="DD" required>
        <input id="startHour" v-model.number="startHour" type="number" placeholder="Military time start hour: HH" required>
        <input id="startMinute" v-model.number="startMinute" type="number" placeholder="Military time start minute: MM"
            required>
        <input id="endHour" v-model.number="endHour" type="number" placeholder="Military time end hour: HH" required>
        <input id="endMinute" v-model.number="endMinute" type="number" placeholder="Military time end minute: MM" required>
        <button type="submit" class="pure-button-primary pure-button">Create Event</button>
    </form>
    <button @click="emit('addEvent')" class="pure-button-primary pure-button">Cancel</button>
</template>

<style scoped>
form {
    background-color: var(--base-bg);
    border-radius: 1em;
    display: flex;
    flex-direction: column;
    gap: 0.5em;
    padding: 1em;
}

textarea {
    font-family: inherit;
    font-size: inherit;
    height: 6em;
    padding: 0.5em;
    border-radius: 4px;
    resize: none;
}
</style>
