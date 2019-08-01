<template>
  <v-container>
    <v-layout>
      <v-flex xs12 sm12 md6>
        <v-text-field label="Book ID" placeholder="Enter book ID" outlined v-model="id"></v-text-field>
        <v-btn color="green" @click="find">Find</v-btn>
      </v-flex>
    </v-layout>
    <br />
    <v-layout v-if="book">
      <v-flex xs12 sm12 md6>
        <p>
          <b>Title:</b>
          {{book.title}}
        </p>
        <p>
          <b>Pages:</b>
          {{book.pages}}
        </p>
        <p>
          <b>Chapters:</b>
          {{book.chapters}}
        </p>
        <p>
          <b>Authors:</b>
          {{ getAuthorNames(book.authors) }}
        </p>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
import gql from "graphql-tag";
export default {
  data: function() {
    return {
      book: null,
      id: ""
    };
  },
  methods: {
    async find() {
      const response = await this.$apollo.query({
        query: gql`
          query($id: ID!) {
            book(id: $id) {
              title
              pages
              chapters
              authors {
                name
              }
            }
          }
        `,
        variables: {
          id: this.id
        }
      });
      this.book = response.data.book;
    },
    getAuthorNames(authors) {
      if (authors.length > 1)
        return authors.reduce((acc, cur) => acc.name + ", " + cur.name);
      else return authors[0].name;
    }
  }
};
</script>
