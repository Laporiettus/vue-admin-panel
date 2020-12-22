<template>
    <tr class="text-center">
        <td @click="editUser(fullName, 'fullName', id)">{{fullName}}</td>
        <td @click="editUser(email, 'email', id)">{{email}}</td>
        <td @click="editUser(password, 'password', id)">{{password}}</td>
        <td @click="editUser(userStatus, 'userStatus', id)">{{userStatus}}</td>
        <td @click="editUser(telephone, 'telephone', id)">{{telephone}}</td>
        <td>{{creationDate}}</td>
        <td>{{lastChangedDate}}</td>
        <td
                @click="deleteUser(id)">
            Удалить пользователя
        </td>
    </tr>
</template>

<script>
    export default {
        name: "Users",
        created() {
            console.log(this.user)
        },
        data() {
            return {
                newInfo: '',
                isEditing: false
            }
        },
        props: {
            users: Array,
            fullName: {
                type: String
            },
            userStatus: {
                type: String
            },
            email: {
                type: String,
                reg: /.+@.+\..+/
            },
            password: {
                type: String,
                required: true
            },
            telephone: {
                type: Number,
                maxlength: 10
            },
            creationDate: {
                type: String
            },
            lastChangedDate: {
                type: String
            },
            user: {
                type: Object
            },
            id: {
                type: Number
            }
        },
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
