<template>
  <v-container fluid fill-height>
    <v-layout align-center justify-center>
      <v-flex xs12 sm8 md4>
        <v-card class="elevation-12">
          <v-toolbar color="primary" dark flat>
            <v-toolbar-title>Login</v-toolbar-title>
          </v-toolbar>
          <v-card-text>
            <v-form>
              <v-text-field v-model="email" label="Email" name="email" type="text"></v-text-field>
              <v-text-field
                v-model="password"
                id="password"
                label="Password"
                name="password"
                type="password"
              ></v-text-field>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn @click="signin" color="primary">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
import { onLogin } from "../vue-apollo.js";
import gql from "graphql-tag";
export default {
  data() {
    return {
      email: "",
      password: ""
    };
  },
  methods: {
    async signin() {
      try {
        const response = await this.$apollo.mutate({
          mutation: gql`
            mutation($email: String!, $password: String!) {
              signin(email: $email, password: $password) {
                token
              }
            }
          `,
          variables: {
            email: this.email,
            password: this.password
          }
        });
        await onLogin(
          this.$apollo.provider.defaultClient,
          response.data.signin.token
        );
        window.location.replace("/");
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

