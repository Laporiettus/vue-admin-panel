<template>
    <tr class="text-center">
        <td
                v-for="(val, propname) in user"
                :key="propname"
        >{{val}}</td>
        <td
                @click="editUserNew(user, id)"
        >Редактировать пользователя</td>
        <td
                @click="deleteUser(id)"
        >Удалить пользователя</td>
    </tr>
</template>

<script>
    export default {
        name: "UserItem",
        props: {
            id: {
                type: Number
            },
            user: {
                type: Object
            }
        },
        methods: {
            editUserNew(user, id) {
                console.log(user, id)
                this.$emit('on-user-edit', {user, id})
            },
            deleteUser(id) {
                this.$emit('on-delete-user', id)
            },
            filterData(val) {
              this.$emit('on-data-filter', val)
            }
        }
    }
</script>

<style lang="scss" scoped>
    ::v-deep td:first-child {
        border-left: 1px solid rgba(179, 8, 8, 0.52)
    }

    ::v-deep td {
        border-top: 1px solid rgba(179, 8, 8, 0.52);
        border-bottom: 1px solid rgba(179, 8, 8, 0.52);
        border-right: 1px solid rgba(179, 8, 8, 0.52)
    }

    ::v-deep table {
        text-align: center;
    }
</style>
