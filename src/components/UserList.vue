<template>
    <div>
        <header-menu
                :toggleNewUser="toggleNewUser"
                @on-toggle-new-user="openNewUser($event)"
                @on-toggle-filter-data="openFilter($event)"
                style="margin-bottom: 40px"
        />
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
                <users
                    v-for="(user, id) in users"
                    :key="id"
                    :user="user"
                    :id="id"
                    :full-name="user.fullName"
                    :email="user.email"
                    :user-status="user.userStatus"
                    :telephone="user.telephone"
                    :password="user.password"
                    :creation-date="user.creationDate"
                    :last-changed-date="user.lastChangeDate"
                    @on-info-edit="editUser($event)"
                    @on-delete-user="deleteUser($event)"
                />
            </tbody>
        </v-simple-table>
        <filter-component
                :users="users"
                v-if="toggleFilter"
                @on-apply-filter="filterData($event)"
                @on-clear-filter="clearFilter($event)"
        />
        <new-user
                v-if="toggleNewUser"
                @on-new-user="createUser($event)"
        />
        <edit-user
                v-if="isEditing"
                :sendingData="sendingData"
                @on-confirm-change="confirmChange($event)"
        />
    </div>
</template>

<script>
    import Users from "./Users";
    import NewUser from "./NewUser";
    import FilterComponent from "./FilterComponent";
    import HeaderMenu from "./headerMenu";
    import EditUser from "./EditUser";

    export default {
        name: "UserList",
        components: {
            HeaderMenu,
            FilterComponent,
            Users,
            NewUser,
            EditUser
        },
        props: {
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
            telephone: {
                type: Number,
                maxlength: 10
            },
            creationDate: {
                type: Date || String
            },
            lastChangedDate: {
                type: Date || String
            },
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
                        creationDate: new Date(2018, 2, 15, 14, 13),
                        lastChangeDate: new Date(2018, 2, 15, 14, 13)
                    },
                    {
                        fullName: 'Борисов Борис Борисович',
                        email: 'bor256@fdfsd.ru',
                        password: '123',
                        userStatus: 'user',
                        telephone: 89497471515,
                        creationDate: new Date(2018, 2, 15, 14, 13),
                        lastChangeDate: new Date(2018, 2, 15, 14, 13)
                    },
                    {
                        fullName: 'Васильева Василиса Васильевна',
                        email: 'Vasilyeva1981@fdfsd.ru',
                        password: '123',
                        userStatus: 'client',
                        telephone: 89211112233,
                        creationDate: new Date(2018, 2, 15, 14, 13),
                        lastChangeDate: new Date(2018, 2, 15, 14, 13)
                    },
                    {
                        fullName: 'Геннадьев Геннадий Геннадьевич',
                        email: 'gena66@bgrt.ru',
                        password: '123',
                        userStatus: 'user',
                        telephone: 89772956710,
                        creationDate: new Date(2018, 2, 15, 14, 13),
                        lastChangeDate: new Date(2018, 2, 15, 14, 13)
                    }
                ],
                toggleNewUser: false,
                toggleFilter: false,
                isEditing: false,
                userStorage: [],
                savedUsers: [],
                sendingData: {}
            }
        },
        created() {
            // Фильтр дат
            this.users.forEach(user => user.creationDate = user.creationDate.toLocaleString())
            this.users.forEach(user => user.lastChangeDate = user.lastChangeDate.toLocaleString())
            // Создаём localStorage
            const parsed = JSON.stringify(this.users)
            if (localStorage.getItem('users') == null) {
                localStorage.setItem('storedUsers', parsed)
                localStorage.setItem('users', parsed)
            }
            // Сохраняем массив в старом виде
            this.savedArr = this.users
        },
        mounted() {
            // Если localStorage не пустой, в массив users заносятся данные из него
            if (localStorage.getItem('users')) {
                try {
                    this.users = JSON.parse(localStorage.getItem('users'));
                } catch (e) {
                    localStorage.removeItem('users');
                }
            }
            this.saveUsers()
        },
        beforeUpdate() {
            // Фильтр дат при обновлении массива
            this.users.forEach(user => user.creationDate = user.creationDate.toLocaleString())
            this.users.forEach(user => user.lastChangeDate = user.lastChangeDate.toLocaleString())
            this.saveUsers()
        },
        methods: {
            // Создание пользователя. Принимает эмит из компонента NewUser
            createUser(user) {
                this.users.push({
                    fullName: user.name,
                    email: user.email,
                    password: user.password,
                    userStatus: user.status,
                    telephone: user.telephone,
                    creationDate: user.createdAt,
                    lastChangeDate: user.lastChangedAt
                })
            },
            // Редактирует пользователя по выбранному полю. Принимает эмит из компонента Users
            // Последовательность методов: editUser() из Users => editUser() в UserList =>
            // confirmChanges() в EditUsers => confirmChange() в UserList
            editUser(user) {
                const {newInfo, ind, id} = user
                console.log(newInfo, ind, id)
                this.isEditing = true
                this.sendingData = user
            },
            // Принимает подтверждение для редактирования. Принимает эмит из EditUsers.
            confirmChange(info) {
                const {id, ind, val} = info
                this.$set(this.users[id], ind, val)
                this.users[id].lastChangeDate = new Date(Date.now())
            },
            // Удаляет пользователя. Принимает эмит из компонента Users
            deleteUser(deletedUser) {
                console.log(deletedUser)
                const confirmation = confirm('Вы действительно хотите удалить этого пользователя?')
                confirmation === true ? this.users.splice(deletedUser, 1) : undefined
            },
            // Фильтр по email или телефону. Эмит из компонента FilterComponent
            filterData(info) {
                const {key, val} = info
                console.log(key, val)
                if (key === 'Email' && val !== '') {
                    this.users = this.users.filter(user => user.email.includes(val))
                    console.log(this.users)
                } else if (key === 'Телефон' && val !== '') {
                    this.users = this.users.filter(user => user.telephone.toString().includes(val))
                }
            },
            // Обновляет localStorage
            saveUsers() {
                const parsed = JSON.stringify(this.users);
                localStorage.setItem('users', parsed);
            },
            // Очищает фильтр и возвращает сохранённый массив. Эмит из FilterComponent
            clearFilter() {
                this.users = this.savedArr
            },
            // Показывает компонент NewUser, закрывает FilterComponent
            openNewUser() {
                this.toggleNewUser = !this.toggleNewUser
                this.toggleFilter = false
            },
            // Показывает компонент FilterComponent, закрывает NewUser
            openFilter() {
                this.toggleFilter = !this.toggleFilter
                this.toggleNewUser = false
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
