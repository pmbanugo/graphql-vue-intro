<template>
  <ul>
    <li>
      <a
        href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel"
        target="_blank"
        rel="noopener"
      >Books</a>
    </li>
    <li v-if="isAuthenticated">
      <a @click="logout">Logout</a>
    </li>
    <li v-else>
      <a @click="login">Login</a>
    </li>
  </ul>
</template>
<script>
import { onLogout, onLogin } from "../vue-apollo";
export default {
  data: function() {
    return {
      isAuthenticated: localStorage.getItem("apollo-token") !== null
    };
  },
  methods: {
    logout: () => {
      this.isAuthenticated = false;
      onLogout();
    },
    login: () => {
      alert("hey oo");
      onLogin();
    }
  }
};
</script>
