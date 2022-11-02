<template>
  <div>
    <Post :postData="postData[0]" />
    <Post :postData="postData[1]" />
    <Post :postData="postData[2]" />
  </div>
</template>

<script>
import Post from "../components/Post.vue";
export default {
  data() {
    return {};
  },
  components: {
    Post,
  },
  props: {
    postData: Array,
  },
};
</script>

<style></style>
