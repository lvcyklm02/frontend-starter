<script setup lang="ts">
import { ref } from "vue";
import { fetchy } from "../../utils/fetchy";

const content = ref("");
const techniques = ref("");
const emit = defineEmits(["refreshPosts"]);

const createPost = async (content: string, techniques: string) => {
  let response;
  try {
    response = await fetchy("api/posts", "POST", {
      body: { content },
    });
  } catch (_) {
    return;
  }

  const technique_list = techniques.split(" ");
  technique_list.forEach((technique) => {
    createTechnique(technique, response.post._id);
  });

  emit("refreshPosts");
  emptyForm();
};

const createTechnique = async (content: string, post_id: string) => {
  try {
    await fetchy(`api/techniques/${post_id}`, "POST", {
      body: { content },
    });
  } catch (_) {
    return;
  }
}

const emptyForm = () => {
  content.value = "";
  techniques.value = "";
};
</script>

<template>
  <form @submit.prevent="createPost(content, techniques)">
    <label for="content">Post Contents:</label>
    <textarea id="content" v-model="content" placeholder="Caption the post!" required> </textarea>
    <textarea id="techniques" v-model="techniques" placeholder="Tag techniques (seperate by space)!"> </textarea>
    <button type="submit" class="pure-button-primary pure-button">Create Post</button>
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
