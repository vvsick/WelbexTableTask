<template>
    <div>
        
        <FilterModule @updateData="changeFilters" />
        
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
        <PaginationModule :content="filteredContent" @onChange="changeInfo"/>
    </div>
</template>

<script>
import PaginationModule from "./PaginationModule/PaginationModule.vue";
import FilterModule from "./FilterModule/FilterModule.vue";
export default {
    data: () => {
        return {
            columns: ['Дата', 'Название', 'Количество', 'Расстояние'],
            info: [
                {date: '12.06.10', title: 'deb', amount: '16', distance: '20'},
                {date: '15.02.13', title: 'poo', amount: '20', distance: '16'},
                {date: '07.03.09', title: 'foo', amount: '3', distance: '10'},
                {date: '23.10.15', title: 'bar', amount: '25', distance: '23'},
                {date: '12.06.10', title: 'deb', amount: '16', distance: '1'},
                {date: '15.02.13', title: 'poo', amount: '20', distance: '1'},
                {date: '07.03.09', title: 'foo', amount: '3', distance: '1'},
                {date: '23.10.15', title: 'bar', amount: '25', distance: '1'},   
            ],
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
                        default: return this.info;
                    }
                    case 'amount' : switch(this.selectedCondition) {
                        case 'equals' : return element.amount === curFilter;
                        case 'contains' : return element.amount.includes(curFilter) ;
                        case 'larger' : return Number(element.amount) > Number(curFilter);
                        case 'less' : return Number(element.amount) < Number(curFilter);
                        default: return this.info;
                    }
                    case 'distance' : switch(this.selectedCondition) {
                        case 'equals' : return element.distance === curFilter;
                        case 'contains' : return element.distance.includes(curFilter) ;
                        case 'larger' : return Number(element.distance) > Number(curFilter);
                        case 'less' : return Number(element.distance) < Number(curFilter);
                        default: return this.info;
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