<template>
  <div class="tag">
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length" class="layout">
      <PostList :posts="postsWithTags"/>
      <TagCloud :posts="posts"/>
    </div>
    <div v-else><Spinner /></div>
  </div>
</template>

<script>
import { computed } from "vue";
import getPosts from "../composables/getPosts";
import { useRoute } from "vue-router";
import PostList from "../components/PostList.vue";
import TagCloud from "../components/TagCloud.vue";
import Spinner from "../components/Spinner.vue";

export default {
  props: ["tags"],
  components: { PostList, Spinner, TagCloud },
  setup() {
    const route = useRoute();
    const { posts, error, load } = getPosts();
    load();
    const postsWithTags = computed(()=>{
       return posts.value.filter((post) => post.tags.includes(route.params.tag));
    })
    return { posts, error, postsWithTags };
  },
};
</script>

<style>
.tag{
    max-width: 1200px;
    margin:0 auto;
    padding: 10px;
}
</style>