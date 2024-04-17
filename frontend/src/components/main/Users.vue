<template>
  <div class="users datatable">
    <div class="caption">User</div>
    <table>
      <thead>
      <tr>
        <th>Id</th>
        <th>Login</th>
        <th>Name</th>
      </tr>
      </thead>
      <tbody>
      <tr v-if="users.length === 0">
        <td colspan="3">No data</td>
      </tr>
      <tr v-for="user in users" :key="user.id">
        <td v-show="users.length !== 0">{{ user.id }}</td>
        <td v-show="users.length !== 0">{{ user.login }}</td>
        <td v-show="users.length !== 0">{{ user.name }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "Index",
  props: ["users"]
};
</script>

<style scoped>

</style>