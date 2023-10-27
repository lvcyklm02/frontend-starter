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
    <div class="technique-label">
        <span class="label-text">{{ props.technique.content }}</span>
        <button class="delete-button" @click="deleteTechnique">X</button>
    </div>
</template>

<style scoped>
.technique-label {
    display: flex;
    align-items: center;
    background-color: #f0f0f0;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin: 10px;
    font-family: Arial, sans-serif;
}

.label-text {
    flex: 1;
    /* Allow the label text to grow and take up available space */
    font-weight: bold;
}

.delete-button {
    background-color: #ff3333;
    color: #fff;
    border: none;
    border-radius: 5px;
    padding: 5px 10px;
    cursor: pointer;
    transition: background-color 0.3s;
}

.delete-button:hover {
    background-color: #cc0000;
}
</style>
