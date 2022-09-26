<template>
    <div class="container mb-5 pt-3">    
        <form @submit.prevent="filter">
            <div class="field has-addons">
                <div class="control">
                    <div class="select">
                        <select
                            name="colFilter"
                            @input="changeColumn"
                            v-model="column"
                            required>
                            <option disabled :value="null">Поле фильтрации</option>
                            <template v-for="column in columns">
                                <option
                                    v-if="column.filter"
                                    :key="column.field" 
                                    :value="column">
                                {{ column.label }}
                                </option>
                            </template>
                        </select>
                    </div>
                </div>
                
                <div class="control">
                    <div class="select">
                        <select
                            name="conditionFilter" 
                            v-model="condition"
                            required>
                            <option disabled :value="null">Условие Фильтрации</option>
                            <option value="equals">Равно</option>
                            <option value="contains">Содержит</option>
                            <option value="larger" v-if="activeColumnIsNumber">Больше</option>
                            <option value="less" v-if="activeColumnIsNumber">Меньше</option>
                        </select>
                    </div>
                </div>
            
                <div class="control is-expanded">
                    <input
                        class="input"
                        :type="activeColumnIsNumber ? 'number' : 'text'" 
                        name="filter"
                        placeholder="Введите значение (только цифры и буквы)" 
                        v-model="value"
                        pattern="^[0-9a-zA-Z]+$">
                </div>
            </div>

            <div class="level-item is-flex is-align-items-center">
                <input type="submit" value="Фильтр"  class="button mr-3"/>
                <button @click="clean" class="button">Сброс</button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        columns: {
            type: Array,
            default: () => ([]),
        },
    },
    data: () => ({
        column: null,
        condition: null,
        value: null,
    }),
    computed: {
        activeColumnIsNumber() {
            return this.column && this.column.type === 'number';
        },
    },
    methods: {
        filter() {
            this.$emit("filter", {
                value: this.value,
                condition: this.condition,
                column: this.column,
            });
        },
        clean() {
            this.column = null;
            this.condition = null;
            this.value = null;
            this.filter();
        },
        changeColumn() {
            this.condition = null;
            this.value = null;
        },
    }
}
</script>