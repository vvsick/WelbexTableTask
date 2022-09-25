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
export default {
    data: () => {
        return {
            columns: ['Дата', 'Название', 'Количество', 'Расстояние'],
            info: [
                {date: '12.06.10', title: 'spool', amount: '21', distance: '20'},
                {date: '12.06.10', title: 'blast', amount: '17', distance: '16'},
                {date: '12.06.10', title: 'gaffe', amount: '16', distance: '10'},
                {date: '13.06.10', title: 'bacon', amount: '25', distance: '23'},
                {date: '13.06.10', title: 'vegetable', amount: '16', distance: '3'},
                {date: '13.06.10', title: 'usher', amount: '20', distance: '46'},
                {date: '13.07.10', title: 'nonbeliever', amount: '3', distance: '40'},
                {date: '13.07.10', title: 'skate', amount: '27', distance: '1'},
                {date: '17.07.11', title: 'gaffe', amount: '3', distance: '19'},
                {date: '17.07.11', title: 'gum', amount: '25', distance: '1'},
                {date: '18.07.11', title: 'synod', amount: '16', distance: '20'},
                {date: '18.07.11', title: 'usher', amount: '20', distance: '16'},
                {date: '18.07.11', title: 'baggy', amount: '3', distance: '10'},
                {date: '18.07.11', title: 'metro', amount: '12', distance: '23'},
                {date: '18.07.11', title: 'sewer', amount: '16', distance: '5'},
                {date: '19.07.11', title: 'congo', amount: '17', distance: '3'},
                {date: '19.07.11', title: 'award', amount: '27', distance: '1'},
                {date: '19.07.11', title: 'sheet', amount: '31', distance: '5'},
                {date: '19.08.11', title: 'width', amount: '12', distance: '23'},
                {date: '19.08.11', title: 'elver', amount: '16', distance: '46'},
                {date: '21.08.13', title: 'swamp', amount: '20', distance: '16'},
                {date: '21.08.13', title: 'flock', amount: '3', distance: '10'},
                {date: '21.08.13', title: 'toast', amount: '25', distance: '23'},
                {date: '21.08.13', title: 'swamp', amount: '21', distance: '3'},
                {date: '21.08.13', title: 'elver', amount: '20', distance: '19'},
                {date: '22.08.13', title: 'toast', amount: '3', distance: '1'},
                {date: '22.08.13', title: 'swamp', amount: '25', distance: '40'},
                {date: '22.08.13', title: 'width', amount: '16', distance: '20'},
                {date: '22.08.13', title: 'metro', amount: '20', distance: '16'},
                {date: '23.08.13', title: 'sewer', amount: '21', distance: '10'},
                {date: '23.08.13', title: 'congo', amount: '31', distance: '23'},
                {date: '23.08.13', title: 'award', amount: '16', distance: '40'},
                {date: '23.06.13', title: 'sheet', amount: '20', distance: '5'},  
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