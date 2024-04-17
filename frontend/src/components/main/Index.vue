<template>
    <div>
        <IndexPost v-for="post in posts" :post="post" :key="post.id"/>
    </div>
</template>

<script>
import IndexPost from "@/components/main/IndexPost.vue";

export default {
    name: "Index",
    props: ["posts"],
  components: {
      IndexPost
  }
}
</script>

<style scoped>

</style>