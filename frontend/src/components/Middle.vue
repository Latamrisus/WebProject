<template>
    <div class="middle">
        <Sidebar :posts="viewPosts"/>
        <main>
            <Index :posts="viewAllPosts" v-if="page === 'Index'"/>
            <Enter v-if="page === 'Enter'"/>
            <Register v-if="page === 'Register'"/>
            <Users :users="viewUsers" v-if="page === 'Users'" />
        </main>
    </div>
</template>

<script>
import Sidebar from "./sidebar/Sidebar";
import Index from "./main/Index";
import Enter from "./main/Enter";
import Register from "./main/Register";
import Users from "./main/Users";

export default {
    name: "Middle",
    data: function () {
        return {
            page: "Index"
        }
    },
    components: {
        Users,
        Register,
        Enter,
        Index,
        Sidebar
    },
    props: ["posts", "users"],
    computed: {
        viewPosts: function () {
            return Object.values(this.posts).sort((a, b) => b.id - a.id).slice(0, 2);
        },
        viewAllPosts: function () {
            return Object.values(this.posts);
        },
        viewUsers: function () {
            return Object.values(this.users);
        }
    }, beforeCreate() {
        this.$root.$on("onChangePage", (page) => this.page = page)
    }
}
</script>

<style scoped>

</style>
