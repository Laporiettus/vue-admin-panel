<template>
    <div>
        <users
                :users="users"
                @on-info-edit="editUser($event)"
                @on-delete-user="deleteUser($event)"
        ></users>
        <new-user
                v-if="toggleNewUser"
                @on-new-user="createUser($event)"
        />
        <div class="w-100 flex justify-content-center ma-top-40">
            <v-btn
                    elevation="2"
                    outlined
                    @click="toggleNewUser = !toggleNewUser"
            >Создать нового пользователя</v-btn>
        </div>
    </div>
</template>

<script>
    import Users from "./Users";
    import NewUser from "./NewUser";
    export default {
        name: "UserList",
        components: {
            Users,
            NewUser
        },
        data() {
            return {
                users: [
                    {
                        fullName: 'Андреев Андрей Андреевич',
                        email: 'andr222@fdfsd.ru',
                        password: '123',
                        userStatus: 'admin',
                        telephone: 89991231211,
                        creationDate: new Date(2018, 3, 15, 14, 30),
                        lastChangeDate: Date.now() - 18400,
                    },
                    {
                        fullName: 'Борисов Борис Борисович',
                        email: 'bor256@fdfsd.ru',
                        password: '123',
                        userStatus: 'user',
                        telephone: 89497471515,
                        creationDate: new Date(2019, 3, 15, 11, 30),
                        lastChangeDate: Date.now() - 65000,
                    },
                    {
                        fullName: 'Васильева Василиса Васильевна',
                        email: 'Vasilyeva1981@fdfsd.ru',
                        password: '123',
                        userStatus: 'client',
                        telephone: 89211112233,
                        creationDate: new Date(2018, 11, 21, 10, 12),
                        lastChangeDate: Date.now() - 128000,
                    },
                    {
                        fullName: 'Геннадьев Геннадий Геннадьевич',
                        email: 'gena66@bgrt.ru',
                        password: '123',
                        userStatus: 'user',
                        telephone: 89772956710,
                        creationDate: new Date(2019, 5, 3, 22, 5),
                        lastChangeDate: Date.now() - 184,
                    }
                ],
                toggleNewUser: false
            }
        },
        methods: {
            createUser(user) {
                this.users.push({
                    fullName: user.name,
                    email: user.email,
                    password: user.password,
                    userStatus: user.status,
                    telephone: user.telephone,
                    creationDate: user.creationDate,
                    lastChangeDate: user.lastChangeDate
                })
            },
            editUser(user) {
                const { newInfo, ind, id } = user
                console.log(user)
                this.$set(this.users[id], ind, newInfo)
            },
            deleteUser(deletedUser) {
                console.log(deletedUser)
                const confirmation = confirm('Вы действительно хотите удалить этого пользователя?')
                confirmation === true ? this.users.splice(deletedUser, 1) : undefined
            }
        }
    }
</script>

<style scoped>
    .w-100 {
        width: 100%;
    }
    .flex {
        display: flex;
    }
    .justify-content-center {
        justify-content: center;
    }
    .ma-top-40 {
        margin-top: 40px;
    }
</style>
