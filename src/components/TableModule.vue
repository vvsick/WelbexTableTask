<template>
    <div>
        <div class="filter">
            <label for="colFilter">По какой колонке фильтрация</label>
            <select name="colFilter" id="colFilter" v-model="selectedColumn">
                <option disabled value="">Выберите значение</option>
                <option value="title" selected>Название</option>
                <option value="amount">Количество</option>
                <option value="distance">Расстояние</option>
            </select>

            <label for="conditionFilter">Условие фильтрации</label>
            <select name="conditionFilter" id="conditionFilter" v-model="selectedCondition">
                <option disabled value="">Выберите значение</option>
                <option value="equals">Равно</option>
                <option value="contains">Содержит</option>
                <option value="larger" v-if="this.selectedColumn=='amount' | this.selectedColumn=='distance'">Больше</option>
                <option value="less" v-if="this.selectedColumn=='amount' | this.selectedColumn=='distance'">Меньше</option>
            </select>

            <input type="text" name="filter" placeholder="Введите значение" v-model="inputValue">
        </div>
        <table>
            <thead>
                <tr>
                    <th v-for="column in columns" :key="column.id">
                        {{column}}
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(element, index) in filteredContent" :key="index">
                    <td v-for="(item, index) in element" :key="index">
                        {{item}}
                    </td>
                </tr>
            </tbody>
        </table>
        <PaginationModule v-bind="content"/>
    </div>
</template>

<script>
import PaginationModule from "./PaginationModule.vue";
export default {
    data: () => {
        return {
            columns: ['Дата', 'Название', 'Количество', 'Расстояние'],
            content: [
                {date: '12.06.10', title: 'deb', amount: '16', distance: '20'},
                {date: '15.02.13', title: 'poo', amount: '20', distance: '16'},
                {date: '07.03.09', title: 'foo', amount: '3', distance: '10'},
                {date: '23.10.15', title: 'bar', amount: '25', distance: '23'},
                {date: '12.06.10', title: 'deb', amount: '16', distance: '20'},
                {date: '15.02.13', title: 'poo', amount: '20', distance: '16'},
                {date: '07.03.09', title: 'foo', amount: '3', distance: '10'},
                {date: '23.10.15', title: 'bar', amount: '25', distance: '23'},   
            ],
            inputValue: '',
            selectedCondition: '',
            selectedColumn: '',
        };
    },
    components: {
    PaginationModule
},
    computed: {
        filteredContent() {
            const curFilter = this.inputValue;
            if(curFilter !== '') {
            return this.content.filter(element => {
                switch(this.selectedColumn) {
                    case 'title' : switch(this.selectedCondition) {
                        case 'equals' : return element.title === curFilter;
                        case 'contains' : return element.title.includes(curFilter);
                        default: return this.content;
                    }
                    case 'amount' : switch(this.selectedCondition) {
                        case 'equals' : return element.amount === curFilter;
                        case 'contains' : return element.amount.includes(curFilter) ;
                        case 'larger' : return Number(element.amount) > Number(curFilter);
                        case 'less' : return Number(element.amount) < Number(curFilter);
                        default: return this.content;
                    }
                    case 'distance' : switch(this.selectedCondition) {
                        case 'equals' : return element.distance === curFilter;
                        case 'contains' : return element.distance.includes(curFilter) ;
                        case 'larger' : return Number(element.distance) > Number(curFilter);
                        case 'less' : return Number(element.distance) < Number(curFilter);
                        default: return this.content;
                    }
                    default: return this.content;
                }
            })
            } else return this.content;
        }
    },
}
</script>

