<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Vuetify</span>
        <span class="font-weight-light">MATERIAL DESIGN</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text href="/">
        <span class="mr-2">Book List</span>
      </v-btn>
      <v-btn text href="/find">
        <span class="mr-2">Find Book</span>
      </v-btn>
      <v-btn text href="/create">
        <span class="mr-2">Create Book</span>
      </v-btn>
      <v-btn v-if="isAuthenticated" color="green" @click="signout">Sign Out</v-btn>
      <v-btn v-else color="green" href="/signin">Sign In</v-btn>
    </v-app-bar>

    <v-content>
      <br />
      <List v-if="currentRoute === '/'" />
      <Create v-if="currentRoute === '/create'" />
      <Find v-if="currentRoute === '/find'" />
      <SignIn v-if="currentRoute === '/signin'" />
    </v-content>
  </v-app>
</template>

<script>
import SignIn from "./components/SignIn";
import List from "./components/List";
import Find from "./components/Find";
import Create from "./components/Create";

import { onLogout } from "./vue-apollo.js";

export default {
  name: "App",
  components: {
    List,
    Find,
    Create,
    SignIn
  },
  data: () => ({
    isAuthenticated: localStorage.getItem("apollo-token") !== null,
    currentRoute: window.location.pathname
  }),
  methods: {
    async signout() {
      await onLogout(this.$apollo.provider.defaultClient);
      window.location.reload();
    }
  }
};
</script>
