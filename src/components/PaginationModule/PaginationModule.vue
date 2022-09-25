<template>
  <nav>
    <ul class="pagination">
      <li 
      v-for="page in totalPages"
      :key="page"
      @click="changePage(page)">
        <div :class="{ active: pageNumber == page ? true : false }">
          {{page}}
        </div>
      </li>
    </ul>
  </nav>
  </template>
  
  <script>
    export default {
      props: {
        content: {
          type: Array,
          default() {
            return []
          }
        }
      },
      data: () => {
        return {
          perPage: 8,
          pageNumber: 1,
        }
      },
      computed: {
        totalPages() {
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
          return this.content.slice(from, to)
        },
      },
      methods: {
        changePage(page) {
          this.pageNumber = page;
        },
  },
  watch: {
    paginatedContent() {
      this.$emit('onChange', this.paginatedContent)
    }
  }
}
  </script>

<style src="./Paginator.css"></style>