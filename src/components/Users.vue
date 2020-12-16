<template>
    <div class="user-table">
        <v-simple-table>
            <thead>
            <tr>
                <th class="text-center">
                    ФИО
                </th>
                <th class="text-center">
                    Email
                </th>
                <th class="text-center">
                    Пароль
                </th>
                <th class="text-center">
                    Статус пользователя
                </th>
                <th class="text-center">
                    Телефон
                </th>
                <th class="text-center">
                    Дата создания
                </th>
                <th class="text-center">
                    Дата последнего изменения
                </th>
            </tr>
            </thead>
            <tbody>
            <tr
                    v-for="(user, id) in users"
                    :key="id"
            >
                <td
                        v-for="(val, key, index) in user"
                        :key="index"
                        @click="editUser(val, key, id)"
                        >
                        {{val}}
                </td>
                <td
                    @click="deleteUser(id)">
                    Удалить пользователя
                </td>
            </tr>
            </tbody>
        </v-simple-table>
    </div>
</template>

<script>
    export default {
        name: "Users",
        data() {
            return {
                newInfo: '',
                isEditing: false,
                userStatus: [
                    'user', 'client', 'admin'
                ],
            }
        },
        props: ['users'],
        methods: {
            editUser(val, ind, id) {
                console.log(val, ind, id)
                if (ind === 'creationDate' || ind === 'lastChangeDate') {
                    return alert('Нельзя менять даты вручную!')
                } else {
                    this.$emit('on-info-edit', {val, ind, id})
                }
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

    .user-table {
        font-size: 12px;
    }
</style>
