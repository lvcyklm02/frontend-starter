<script setup lang="ts">
import CommentComponent from "@/components/Comment/CommentComponent.vue";
import CreateCommentForm from "@/components/Comment/CreateCommentForm.vue";
import EditCommentForm from "@/components/Comment/EditCommentForm.vue";
import { useUserStore } from "@/stores/user";
import { fetchy } from "@/utils/fetchy";
import { storeToRefs } from "pinia";
import { onBeforeMount, ref } from "vue";
// import SearchPostForm from "./SearchPostForm.vue";

const { isLoggedIn } = storeToRefs(useUserStore());

const props = defineProps(["post"]);
const loaded = ref(false);
let comments = ref<Array<Record<string, string>>>([]);
let editing = ref("");

async function getComments() {
    let commentResults;
    try {
        commentResults = await fetchy(`api/comments/${props.post._id}`, "GET");
    } catch (_) {
        return;
    }
    comments.value = commentResults;
}

function updateEditing(id: string) {
    editing.value = id;
}

onBeforeMount(async () => {
    await getComments();
    loaded.value = true;
});
</script>

<template>
    <h2>Comments: </h2>
    <section class="comments" v-if="loaded && comments.length !== 0">
        <article v-for="comment in comments" :key="comment._id">
            <CommentComponent v-if="editing !== comment._id" :comment="comment" @refreshComments="getComments"
                @editComment="updateEditing" />
            <EditCommentForm v-else :comment="comment" @refreshComments="getComments" @editComment="updateEditing" />
        </article>
    </section>
    <p v-else-if="loaded">No comments yet</p>
    <p v-else>Loading...</p>
    <section v-if="isLoggedIn">
        <CreateCommentForm :post="post" @refreshComments="getComments" />
    </section>
</template>

<style scoped>
section {
    display: flex;
    flex-direction: column;
    gap: 1em;
}

article {
    background-color: var(--base-bg);
    border-radius: 1em;
    display: flex;
    flex-direction: column;
    gap: 0.5em;
    padding: 1em;
}

.comments {
    padding: 1em;
}
</style>
