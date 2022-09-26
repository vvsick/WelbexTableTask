<template>
    <aside class="pagination is-centered mx-3 mt-5">
        
        <button @click="prev" type="button" class="pagination-previous"> Назад </button>
        <button @click="next" type="button" class="pagination-next"> Вперед </button>
        

        <div class="pagination-list">
            <a 
                class="pagination-link"
                :class="{ 'pagination-link is-current': page === 1 }"
                @click="setPage(1)">
                1
            </a>
            <template v-if="page - 1 > 2">
                <div class="pagination-ellipsis">...</div>
            </template>
            <template>
                <a
                    v-for="item in rangePages"
                    :key="item"
                    class="pagination-link"
                    :class="{ 'pagination-link is-current': page === item }"
                    @click="setPage(item)">
                    {{ item }}
                </a>
            </template>
            <template v-if="countPages === null || countPages - page > 2">
                <div class="pagination-ellipsis">...</div>
            </template>
            <a 
                v-if="countPages && countPages !== 1"
                class="pagination-link"
                :class="{ 'pagination-link is-current': page === countPages }"
                @click="setPage(countPages)"
            >
                {{ countPages }}
            </a>
        </div>
    </aside>
</template>

<script>

export default {
    props: {
        total: {
            type: Number,
            default: null,
        },
        page: {
            type: Number,
            required: true,
        },
        perPage: {
            type: Number,
            default: null,
        }
    },
    computed: {
        countPages() {
            if (this.total === null || this.perPage === null) return null;
            return Math.ceil(this.total / this.perPage);
        },
        rangePages() {
            const result = [this.page - 1, this.page, this.page + 1];
            if (result[0] <= 1) result.shift();
            if (result[0] <= 1) result.shift();
            if (this.countPages !== null && result[result.length - 1] >= this.countPages) result.pop(); 
            if (this.countPages !== null && result[result.length - 1] >= this.countPages) result.pop();
            return result;
        }
    },
    methods: {
        prev() {
            this.setPage(this.page - 1);
        },
        next() {
            this.setPage(this.page + 1);
        },
        setPage(value) {
            let page;
            if (value < 1) page = 1;
            else if (this.countPages !== null && value > this.countPages) page = this.countPages;
            else page = value;
            this.$emit('update:page', page);
        },
    },
};
</script>