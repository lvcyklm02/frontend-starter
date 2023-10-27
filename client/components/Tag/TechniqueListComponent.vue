<script setup lang="ts">
import TechniqueComponent from "@/components/Tag/TechniqueComponent.vue";

import { useUserStore } from "@/stores/user";
import { fetchy } from "@/utils/fetchy";
import { storeToRefs } from "pinia";
import { onBeforeMount, ref } from "vue";

const { isLoggedIn } = storeToRefs(useUserStore());

const props = defineProps(["post"]);
const loaded = ref(false);
let techniques = ref<Array<Record<string, string>>>([]);
let editing = ref("");

async function getTechniques() {
    let techniqueResults;
    try {
        techniqueResults = await fetchy(`api/techniques/${props.post._id}`, "GET");
    } catch (_) {
        return;
    }
    techniques.value = techniqueResults;
}

function updateEditing(id: string) {
    editing.value = id;
}

onBeforeMount(async () => {
    await getTechniques();
    loaded.value = true;
});
</script>

<template>
    <p>Techniques: </p>
    <section class="techniques" v-if="loaded && techniques.length !== 0">
        <article v-for="technique in techniques" :key="technique._id">
            <TechniqueComponent v-if="editing !== technique._id" :technique="technique" @refreshTechniques="getTechniques"
                @editTechnique="updateEditing" />
            <!-- <EditTechniqueForm v-else :technique="technique" @refreshtechniques="getTechniques"
                @editTechnique="updateEditing" /> -->
        </article>
    </section>
    <p v-else-if="loaded">No techniques yet</p>
    <p v-else>Loading...</p>
    <!-- <section v-if="isLoggedIn">
        <CreateCommentForm :post="post" @refreshComments="getComments" />
    </section> -->
</template>

<style scoped>
section {
    display: flex;
    flex-direction: row;
    gap: 1em;
}

article {
    background-color: var(--base-bg);
    border-radius: 1em;
    display: flex;
    flex-direction: row;
    gap: 0.5em;
    padding: 1em;
}

.comments {
    padding: 1em;
}
</style>
