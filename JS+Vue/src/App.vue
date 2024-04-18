<template>
  <div id="app">
    <Header :userId="userId" :users="users"/>
    <Middle :posts="posts" :users="users"/>
    <Footer :userCount="countUsers" :post-count="countPosts"/>
  </div>
</template>

<script>
import Header from "./components/Header";
import Middle from "./components/Middle";
import Footer from "./components/Footer";

export default {
  name: 'App',
  components: {
    Footer,
    Middle,
    Header
  },
  data: function () {
    return this.$root.$data;
  },
  computed: {
    countUsers: function () {
      return Object.keys(this.users).length;
    },
    countPosts: function () {
      return Object.keys(this.posts).length;
    },
  },
  beforeCreate() {
    this.$root.$on("onEnter", (login, password) => {
      if (password === "") {
        this.$root.$emit("onEnterValidationError", "Password is required");
        return;
      }

      const users = Object.values(this.users).filter(u => u.login === login);
      if (users.length === 0) {
        this.$root.$emit("onEnterValidationError", "No such user");
      } else {
        this.userId = users[0].id;
        this.$root.$emit("onChangePage", "Index");
      }
    });

    this.$root.$on("onLogout", () => this.userId = null);

    this.$root.$on("onWritePost", (title, text) => {
      if (this.userId) {
        if (!title || title.length < 4) {
          this.$root.$emit("onWritePostValidationError", "Title is too short");
        } else if (!text || text.length < 10) {
          this.$root.$emit("onWritePostValidationError", "Text is too short");
        } else {
          const id = Math.max(...Object.keys(this.posts)) + 1;
          this.$root.$set(this.posts, id, {
            id, title, text, userId: this.userId
          });
        }
      } else {
        this.$root.$emit("onWritePostValidationError", "No access");
      }
    });

    this.$root.$on("onRegister", (login, name) => {
      if (!this.userId) {
        const existingUser = Object.values(this.users).find(u => u.login === login);
        const validLoginPattern = /^[a-z]+$/;
        if (!login || login.length < 3) {
          this.$root.$emit("onRegisterValidationError", "Login is too short");
        } else if (login.length > 16) {
          this.$root.$emit("onRegisterValidationError", "Login is too long");
        } else if (existingUser) {
          this.$root.$emit("onRegisterValidationError", "Login is already taken");
        } else if (!validLoginPattern.test(login)) {
          this.$root.$emit("onRegisterValidationError", "Login should contain only lowercase latin letters");
        } else if (!name || name.trim().length === 0 || name.length < 1) {
          this.$root.$emit("onRegisterValidationError", "Name is required");
        } else if (name > 32) {
          this.$root.$emit("onRegisterValidationError", "Name is too long");
        } else {
          const id = Math.max(...Object.keys(this.users)) + 1;
          this.$root.$set(this.users, id, {
            id, login, name
          });
          this.$root.$emit("onChangePage", "Enter");
        }
      } else {
        this.$root.$emit("onRegisterValidationError", "You are already registered");
      }
    });

    this.$root.$on("onEditPost", (id, text) => {
      if (this.userId) {
        if (!id) {
          this.$root.$emit("onEditPostValidationError", "ID is invalid");
        } else if (!text || text.length < 10) {
          this.$root.$emit("onEditPostValidationError", "Text is too short");
        } else {
          let posts = Object.values(this.posts).filter(p => p.id === parseInt(id));
          if (posts.length) {
            posts.forEach((item) => {
              item.text = text;
            });
          } else {
            this.$root.$emit("onEditPostValidationError", "No such post");
          }
        }
      } else {
        this.$root.$emit("onEditPostValidationError", "No access");
      }
    });
  }
}
</script>

<style>
#app {

}
</style>
