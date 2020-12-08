<template>
    <div class="user-table">
        <v-simple-table>
            <thead>
            <tr>
                <th class="text-center">
                    ФИО
                </th>
                <th class="text-center">
                    Статус пользователя
                </th>
                <th class="text-center">
                    Пароль
                </th>
                <th class="text-center">
                    Email
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
                        v-for="(val, index) in user"
                        :key="index"
                        @click="editUser(val, index, id)"
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
        <v-text-field
                label="Main input"
                v-if="isEditing"
                v-model="newInfo"
        ></v-text-field>
    </div>
</template>

<script>
    export default {
        name: "Users",
        data() {
            return {
                newInfo: '',
                isEditing: false,
            }
        },
        props: ['users'],
        fullName: {
            type: String,
            required: true
        },
        userStatus: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        telephone: {
            type: Number,
            maxlength: 10,
            required: true
        },
        createdAt: {
            type: Date
        },
        lastChangedAt: {
            type: Date
        },
        methods: {
            editUser(val, ind, id) {
                this.isEditing = true;
                this.$emit('on-info-edit', {newInfo: this.newInfo, ind, id})
                if (this.newInfo.length > 0) {
                    this.newInfo = ''
                }
            },
            deleteUser(id) {
                this.$emit('on-delete-user', id)
            }
        },
        filters: {
            dateFormat: d => d.toLocaleString().replace(',', '').slice(0, -3)
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
