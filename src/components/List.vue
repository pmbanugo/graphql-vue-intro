<template>
  <v-simple-table>
    <thead>
      <tr>
        <th class="text-left">Title</th>
        <th class="text-left">Authors</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="book in books" :key="book.title">
        <td>{{ book.title }}</td>
        <td>{{ getAuthorNames(book.authors) }}</td>
      </tr>
    </tbody>
  </v-simple-table>
</template>
<script>
import gql from "graphql-tag";
export default {
  apollo: {
    books: gql`
      {
        books {
          title
          authors {
            name
          }
        }
      }
    `
  },
  methods: {
    getAuthorNames(authors) {
      if (authors.length > 1)
        return authors.reduce((acc, cur) => acc.name + ", " + cur.name);
      else return authors[0].name;
    }
  }
};
</script>
