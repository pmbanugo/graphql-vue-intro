<template>
  <form>
    <v-text-field v-model="title" label="Title" required></v-text-field>
    <v-text-field v-model="pages" label="Pages" required></v-text-field>
    <v-text-field v-model="chapters" label="Chapters" required></v-text-field>
    <v-text-field v-model="authors" label="Authors" required></v-text-field>

    <v-btn color="success" class="mr-4" @click="submit">Submit</v-btn>
  </form>
</template>
<script>
import gql from "graphql-tag";
export default {
  data() {
    return {
      title: "",
      pages: 0,
      chapters: 0,
      authors: ""
    };
  },
  methods: {
    submit() {
      this.$apollo
        .mutate({
          mutation: gql`
            mutation(
              $title: String!
              $pages: Int!
              $chapters: Int!
              $authors: [String!]!
            ) {
              book(
                title: $title
                pages: $pages
                chapters: $chapters
                authors: $authors
              ) {
                id
                title
              }
            }
          `,
          // Parameters
          variables: {
            title: this.title,
            pages: Number.parseInt(this.pages),
            chapters: Number.parseInt(this.chapters),
            authors: this.authors.replace(/ /g, "").split(",")
          }
        })
        .then(response => {
          alert(
            `Book created. ID: ${response.data.book.id}. Title: ${response.data.book.title}`
          );
          this.title = "";
          this.pages = 0;
          this.chapters = 0;
          this.authors = "";
        })
        .catch(error => {
          console.error(error);
          alert("Book could not be saved");
        });
    }
  }
};
</script>
