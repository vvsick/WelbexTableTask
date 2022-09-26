<template>
    <div class="main-wrapper">

        <FilterModule
            :columns="columns"
            @filter="onChangeFilter" />

        <div class="table-wrap mx-4">
            <table class="table is-striped is-fullwidth is-bordered">
                <thead>
                    <tr>
                        <th
                            v-for="column in columns"
                            :key="column.field"
                            class="table-wrap__th"
                            @click="onSort(column)"
                        >
                            <div class="table-wrap__header-column">
                                {{ column.label }}

                                <div 
                                    class="table-wrap__header-sort"
                                    :class="{ 'table-wrap__header-sort_hidden': sort.column !== column }">
                                    <div v-if="sort.order ==='asc'">🔼</div>
                                    <div v-else>🔽</div>
                                </div>
                            </div>
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in items" :key="index">
                        <td>{{ item.date }}</td>
                        <td>{{ item.title }}</td>
                        <td>{{ item.amount }}</td>
                        <td>{{ item.distance }}</td>
                    </tr>
                </tbody>
            </table>
        </div>

        <BasePagination 
            :total="filteredItems.length"
            :perPage="pagination.perPage" 
            :page.sync="pagination.page" />
    </div>
</template>

<script>
import BasePagination from "./BasePagination.vue";
import FilterModule from "./FilterModule/FilterModule.vue";
import { data } from "../Data/data.js";

export default {
    components: {
        FilterModule,
        BasePagination,
    },
    data: () => ({
        data,
        columns: [
            { field: 'date', label: 'Дата', type: 'string', filter: false },
            { field: 'title', label: 'Название', type: 'string', filter: true },
            { field: 'amount', label: 'Количество', type: 'number', filter: true },
            { field: 'distance', label: 'Расстояние', type: 'number', filter: true },
        ],
        filter: {
            column: null,
            value: null,
            condition: null,
        },
        pagination: {
            page: 1,
            perPage: 5,
        },
        sort: {
            column: null,
            order: 'asc',
        }
    }),
    computed: {
        filteredItems() {
            const { column, value, condition } = this.filter;

            if (column === null || value === null || condition === null) return data;
            if (data[0] && !data[0][column.field]) return data;

            return data.filter((item) => {
                const actual = item[column.field];
                if (condition === 'equals') return actual === value;
                if (condition === 'contains') return actual.includes(value);
                if (condition === 'larger') return Number(actual) > Number(value);
                if (condition === 'less') return Number(actual) < Number(value);
            });
        },
        sortedItems() {
            if (this.sort.column === null) return this.filteredItems;
            
            const { column, order } = this.sort;
            return this.filteredItems.slice().sort((a, b) => {
                if (column.type === 'number') {
                    if (order === 'asc') {
                        return a[column.field] - b[column.field];
                    }

                    return b[column.field] - a[column.field];
                } else if (column.type === 'string') {
                    if (order === 'asc') {
                        return a[column.field].localeCompare(b[column.field]);
                    }
                
                    return b[column.field].localeCompare(a[column.field]);
                }

                return 1;
            });
        },
        items() {
            const { page, perPage } = this.pagination;
            const offset = (page - 1) * perPage; 
            return this.sortedItems.slice(offset, offset + perPage);
        }
    },
    methods: {
        onChangeFilter(data) {
            this.pagination.page = 1;
            this.filter.column = data.column;
            this.filter.value = data.value;
            this.filter.condition = data.condition;
        },
        onSort(column) {
            if (this.sort.column === column) {
                if (this.sort.order === 'asc') this.sort.order = 'desc';
                else if (this.sort.order === 'desc') this.sort.column = null;
            } else {
                this.sort.column = column;
                this.sort.order = 'asc';
            }
        },
    }
}
</script>

<style>
    .table-wrap__header-column {
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;
        user-select: none;
        cursor: pointer;
    }
    
    .table-wrap__header-sort {
        position: absolute;
        left: 90%;
        top: 0;
        bottom: 0;
        margin: auto 0;
    }
    .table-wrap__header-sort_hidden {
        opacity: 0;
    }
</style>