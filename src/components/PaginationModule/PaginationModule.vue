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
      props: {
        content: {
          type: Array,
          default() {
            return []
          }
        },
        infoToPaginate: {
          type: Array,
          default() {
            return []
          }
        }
      },
      data: () => {
        return {
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
      this.$emit('onChange', this.content.slice(from, to));
      return ''
    },
  },
  methods: {
    changePage(page) {
      this.pageNumber = page;
    },
  },
  watch: {
    paginatedContent() {
      console.log(this.paginatedContent);
      this.$emit('onChange', this.paginatedContent)
    }
  }
}
  </script>

<style src="./Paginator.css"></style>