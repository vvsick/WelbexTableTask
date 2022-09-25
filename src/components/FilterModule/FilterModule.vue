<template>
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

        <input type="text" name="filter" placeholder="Введите значение" v-model="inputValue" @input="updateData()">
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