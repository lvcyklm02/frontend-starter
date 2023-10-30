<script setup lang="ts">
import CreatePostForm from "@/components/Post/CreatePostForm.vue";
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";

const { currentUsername, isLoggedIn } = storeToRefs(useUserStore());
</script>

<template>
    <main>
        <h1>Upload</h1>
        <section>
            <h1 v-if="isLoggedIn">Welcome {{ currentUsername }}!</h1>
            <h1 v-else>Log in to view</h1>
        </section>
        <CreatePostForm />
    </main>
</template>

<style scoped>
h1 {
    text-align: center;
}
</style>
