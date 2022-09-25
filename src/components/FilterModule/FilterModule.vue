<template>
    <div class="filter-wrap">    
        <form @submit.prevent="updateData()">
            <div class="filter">
                <label for="colFilter">Поле фильтрации
                    <select 
                    name="colFilter" id="colFilter" 
                    v-model="selectedColumn" 
                    required>
                        <option disabled value="">Выберите значение</option>
                        <option value="title" selected>Название</option>
                        <option value="amount">Количество</option>
                        <option value="distance">Расстояние</option>
                    </select>
                </label>

                <label for="conditionFilter">Условие фильтрации
                    <select 
                    name="conditionFilter" 
                    id="conditionFilter" 
                    v-model="selectedCondition"
                    required>
                        <option disabled value="">Выберите значение</option>
                        <option value="equals">Равно</option>
                        <option value="contains">Содержит</option>
                        <option 
                        value="larger" 
                        v-if="this.selectedColumn=='amount' | this.selectedColumn=='distance'">Больше</option>
                        <option 
                        value="less" 
                        v-if="this.selectedColumn=='amount' | this.selectedColumn=='distance'">Меньше</option>
                    </select>
                </label>
                
                <input 
                type="text" 
                name="filter" 
                placeholder="Введите значение (только цифры и буквы)" 
                v-model="inputValue"
                pattern="^[0-9a-zA-Z]+$">
            </div>
                <input type="submit" value="Фильтр" />
        </form>
    </div>
</template>

<script>
export default {
    data: () => {
        return {
            inputValue: '',
            selectedCondition: '',
            selectedColumn: '',
        }
    },
    methods: {
        updateData() {
            this.$emit("updateData", {
                inpValue: this.inputValue,
                condition: this.selectedCondition,
                column: this.selectedColumn
            })
        }
    }
}
</script>

<style src="./Filter.css"></style>