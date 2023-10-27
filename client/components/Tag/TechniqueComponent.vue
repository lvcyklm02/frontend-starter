<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";
import { fetchy } from "../../utils/fetchy";

const props = defineProps(["technique"]);
const emit = defineEmits(["refreshTechniques", "editTechnique"]);
const { currentUsername } = storeToRefs(useUserStore());

const deleteTechnique = async () => {
    try {
        await fetchy(`api/techniques/${props.technique._id}`, "DELETE");
    } catch {
        return;
    }
    emit("refreshTechniques");
};

const sendRefreshPost = async () => {
    emit("refreshTechniques");
};
</script>

<template>
    <div class="tag_button">
        <p>{{ props.technique.content }}</p>
        <button class="button-error btn-small pure-button" @click="deleteTechnique">X</button>
    </div>
</template>

<style scoped>
p {
    margin: 0em;
}

menu {
    list-style-type: none;
    display: flex;
    flex-direction: row;
    gap: 1em;
    padding: 0;
    margin: 0;
}

.tag_button {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: azure;
}
</style>
