<template>
        <v-toolbar class="flex flex-row justify-items">
            <v-btn @click="createUser">Создать пользователя</v-btn>
            <v-btn @click="filterData">Фильтрация данных</v-btn>
        </v-toolbar>
</template>

<script>
    export default {
        name: "HeaderMenu",
        methods: {
            createUser() {
                this.$emit('on-toggle-new-user')
            },
            filterData() {
                this.$emit('on-toggle-filter-data')
            }
        }
    }
</script>

<style scoped>
    .justify-items {
        justify-content: space-evenly;
    }
    /* div in header of v-toolbar */
    ::v-deep .v-toolbar__content {
        max-width: inherit;
        width: 100%;
        justify-content: space-evenly;
    }

</style>
