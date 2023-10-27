<script setup lang="ts">
import { ObjectId } from "mongodb";
import { ref } from "vue";
import { fetchy } from "../../utils/fetchy";

const content = ref("");
const props = defineProps(["post"]);
const emit = defineEmits(["refreshPosts"]);

const createTechnique = async (content: string, root: ObjectId) => {
    const technique = content.split(" ");
    technique.forEach(async (technique) => {
        try {
            await fetchy("api/techniques", "POST", {
                content: { content },
                root: { root },
            });
        } catch (_) {
            return;
        }
    })

    emptyForm();
};

const emptyForm = () => {
    content.value = "";
};
</script>

<template>
    <form @submit.prevent="createTechnique(content, props.post._id)">
        <label for="content">Add Techniques (whitespace seperators):</label>
        <textarea id="content" v-model="content" placeholder="isolation popping kah" required> </textarea>
        <button type="submit" class="pure-button-primary pure-button">Post Comment</button>
    </form>
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
