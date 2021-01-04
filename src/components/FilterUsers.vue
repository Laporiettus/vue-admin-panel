<template>
    <v-form v-model="formValidation">
        <v-text-field
                v-model="filterInfo"
                label="Введите данные для поиска"
        ></v-text-field>
        <v-select
                v-model="itemKey"
                :items="optionsList"
                label="Выберите тип фильтра"
        ></v-select>
        <v-btn
                @click="applyFilter(itemKey, filterInfo)"
        >
            Применить фильтр
        </v-btn>
        <v-btn
                @click="clearFilter()"
                :disabled="!formValidation"
        >
            Сбросить фильтр
        </v-btn>
    </v-form>
</template>

<script>
    export default {
        name: "FilterUsers",
        data() {
            return {
                itemKey: '',
                filterInfo: '',
                formValidation: false,
                optionsList: ['Email', 'Телефон']
            }
        },
        methods: {
            applyFilter(key, val) {
                this.$emit('on-apply-filter', {key, val})
            },
            clearFilter() {
                this.$emit('on-clear-filter', '')
            }
        }
    }
</script>

<style scoped>

</style>
