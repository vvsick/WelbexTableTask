<template>
    <div class="main-wrapper">
        
        <FilterModule @updateData="changeFilters" />
        <div class="table-wrap">
            <table>
                <thead>
                    <tr>
                        <th v-for="column in columns" :key="column.id">
                            {{column}}
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(element, index) in paginatedInfo" :key="index">
                        <td v-for="(item, index) in element" :key="index">
                            {{item}}
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <PaginationModule :content="filteredContent" @onChange="changeInfo"/>
    </div>
</template>

<script>
import PaginationModule from "./PaginationModule/PaginationModule.vue";
import FilterModule from "./FilterModule/FilterModule.vue";
import { data } from "../Data/data.js";
export default {
    data: () => {
        return {
            columns: ['Дата', 'Название', 'Количество', 'Расстояние'],
            info: data,
            paginatedInfo: [],
            inputValue: '',
            selectedCondition: '',
            selectedColumn: '',
        };
    },
    components: {
    PaginationModule,
    FilterModule
},
    computed: {
        filteredContent() {
            const curFilter = this.inputValue;
            if(curFilter !== '') {
            return this.info.filter(element => {
                switch(this.selectedColumn) {
                    case 'title' : switch(this.selectedCondition) {
                        case 'equals' : return element.title === curFilter;
                        case 'contains' : return element.title.includes(curFilter);
                        default: return element.title === curFilter;
                    }
                    case 'amount' : switch(this.selectedCondition) {
                        case 'equals' : return element.amount === curFilter;
                        case 'contains' : return element.amount.includes(curFilter) ;
                        case 'larger' : return Number(element.amount) > Number(curFilter);
                        case 'less' : return Number(element.amount) < Number(curFilter);
                        default: return element.amount === curFilter;
                    }
                    case 'distance' : switch(this.selectedCondition) {
                        case 'equals' : return element.distance === curFilter;
                        case 'contains' : return element.distance.includes(curFilter) ;
                        case 'larger' : return Number(element.distance) > Number(curFilter);
                        case 'less' : return Number(element.distance) < Number(curFilter);
                        default: return element.distance === curFilter;
                    }
                    default: return this.info;
                }
            })
            } else return this.info;
        },
    },
    methods: {
        changeInfo(data) {
            this.paginatedInfo = data;
        },
        changeFilters(data) {
            this.inputValue = data.inpValue;
            this.selectedCondition = data.condition;
            this.selectedColumn = data.column;
        },
    }
}
</script>

<style src="./Table.css"></style>