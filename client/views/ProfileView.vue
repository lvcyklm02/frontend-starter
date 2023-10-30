<script setup lang="ts">
import PostListComponent from "@/components/Post/PostListComponent.vue";
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";

const { currentUsername } = storeToRefs(useUserStore());
</script>

<template>
    <main>
        <div class="profile-header">
            <img class="profile-pic" src="@/assets/images/shoe.ico">
            <div class="profile-header-content">
                <div class="username">{{ currentUsername }}</div>
                <div class="name">name</div>
                <div class="region">region</div>
                <div class="days-on-kah">152 days on kah</div>
            </div>
            <div class="profile-setting">Setting</div>
        </div>
        <div>
            <ul class="account-stats">
                <li>152 Vids</li>
                <li>34 Watchers</li>
                <li>59 Watching</li>
            </ul>
        </div>

        <PostListComponent />
    </main>
</template>

<style scoped>
h1 {
    text-align: center;
}

section {
    display: flex;
    flex-direction: column;
    width: 100%;
    justify-content: center;
}

.profile-header {
    /* margin-left: auto; */
    display: flex;
    align-items: center;
    flex-direction: row;
    justify-content: center;
}

.account-stats {
    list-style-type: none;
    /* margin-left: auto; */
    display: flex;
    align-items: center;
    flex-direction: row;
    gap: 10em;
    justify-content: center;
}

img {
    width: 10em;
}

.profile-header-content {
    width: 20em;
}

.profile-setting {
    width: 10em;
}
</style>
