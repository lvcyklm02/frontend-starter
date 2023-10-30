<script setup lang="ts">
import { useToastStore } from "@/stores/toast";
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";
import { computed, onBeforeMount } from "vue";
import { RouterLink, RouterView, useRoute } from "vue-router";

const currentRoute = useRoute();
const currentRouteName = computed(() => currentRoute.name);
const userStore = useUserStore();
const { isLoggedIn } = storeToRefs(userStore);
const { toast } = storeToRefs(useToastStore());

// Make sure to update the session before mounting the app in case the user is already logged in
onBeforeMount(async () => {
  try {
    await userStore.updateSession();
  } catch {
    // User is not logged in
  }
});
</script>

<template>
  <header>
    <div class="title">
      <img src="@/assets/images/shoe.ico" />
      <RouterLink :to="{ name: 'Feed' }">
        <h1>kah</h1>
      </RouterLink>
    </div>
    <article v-if="toast !== null" class="toast" :class="toast.style">
      <p>{{ toast.message }}</p>
    </article>
  </header>
  <main>
    <RouterView />
  </main>
  <footer>
    <nav>
      <ul>
        <li>
          <RouterLink :to="{ name: 'Feed' }" :class="{ underline: currentRouteName == 'Feed' }"> Feed </RouterLink>
        </li>
        <li v-if="isLoggedIn">
          <RouterLink :to="{ name: 'Explore' }" :class="{ underline: currentRouteName == 'Explore' }"> Explore
          </RouterLink>
        </li>
        <li v-if="isLoggedIn">
          <RouterLink :to="{ name: 'Upload' }" :class="{ underline: currentRouteName == 'Upload' }"> Upload
          </RouterLink>
        </li>
        <li v-if="isLoggedIn">
          <RouterLink :to="{ name: 'Profile' }" :class="{ underline: currentRouteName == 'Profile' }"> Profile
          </RouterLink>
        </li>
        <li v-else>
          <RouterLink :to="{ name: 'Login' }" :class="{ underline: currentRouteName == 'Login' }"> Login </RouterLink>
        </li>
      </ul>
    </nav>
  </footer>
</template>

<style scoped>
@import "./assets/toast.css";

header {
  overflow: hidden;
  position: fixed;
  width: 100%;
  top: 0;
  padding: 1em 2em;
  background-color: lightgray;
  display: flex;
  align-items: center;
}

footer {
  overflow: hidden;
  position: fixed;
  width: 100%;
  bottom: 0;
  padding: 1em 2em;
  background-color: lightgray;
  display: flex;
  align-items: center;
}

main {
  margin-top: 100px;
  margin-bottom: 100px;
}

h1 {
  font-size: 2em;
  margin: 0;
}

.title {
  display: flex;
  align-items: center;
  gap: 0.5em;
}

img {
  height: 2em;
}

a {
  font-size: large;
  color: black;
  text-decoration: none;
}

ul {
  list-style-type: none;
  margin-left: auto;
  display: flex;
  align-items: center;
  flex-direction: row;
  gap: 1em;
}

.underline {
  text-decoration: underline;
}
</style>
