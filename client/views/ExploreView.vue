<script setup lang="ts">
import PostExploreComponent from "@/components/Post/PostExploreComponent.vue";
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";

const { currentUsername, isLoggedIn } = storeToRefs(useUserStore());
</script>

<template>
    <main>
        <h1>Explore</h1>
        <section>
            <h1 v-if="isLoggedIn">Welcome {{ currentUsername }}!</h1>
            <h1 v-else>Log in to view</h1>
        </section>
        <PostExploreComponent />
    </main>
</template>

<style scoped>
h1 {
    text-align: center;
}
</style>
