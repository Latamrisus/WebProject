<template>
    <footer>
      <a href="/">Codeforces</a> &copy; 2022 by Mike Mirzayanov
      <p>Registered users: {{ userCount }}</p>
      <p>Number of posts: {{ postCount }}</p>
    </footer>
</template>

<script>
export default {
    name: "Footer",
    props: ["userCount", "postCount"]
}
</script>

<style scoped>

</style>