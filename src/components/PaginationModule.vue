<template>
  <div class="pagination is-centered">
    <div class="pagination-list">
      <div class="pagination-link" 
      v-for="page in pages"
      :key="page"
      @click="changePage(page)">
      {{page}}</div>
    </div>
  </div>
  </template>
  
  <script>
    export default {
      props: ['content'],
      data: () => {
        return {
          content: this.content,
          perPage: 4,
          pageNumber: 1,
        }
      },
      computed: {
    pages() {
      let count = 0;
      for (let key = 1; key <= this.content.length; key++) {
        count++;
      }
      return Math.ceil(count / this.perPage);
    },
    paginatedContent() {
      let from = (this.pageNumber - 1) * this.perPage;
      let to = from + this.perPage;
      return this.content.slice(from, to);
    },
  },
  methods: {
    changePage(page) {
      this.pageNumber = page;
    },
  },
}
  </script>