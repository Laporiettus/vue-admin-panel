<template>
    <div>
        <HeaderMenu
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
                <UserItem
                    v-for="(user, id) in users"
                    :key="id"
                    :id="id"
                    :user="user"
                    @on-delete-user="deleteUser($event)"
                    @on-user-edit="editUser($event)"
                />
            </tbody>
        </v-simple-table>
        <FilterUsers
                :users="users"
                v-if="toggleFilter"
                @on-apply-filter="filterData($event)"
                @on-clear-filter="clearFilter($event)"
        />
        <NewUser
                v-if="toggleNewUser"
                @on-new-user="createUser($event)"
        />
        <EditUser
                v-if="isEditing"
                :sendingData="sendingData"
                @on-confirm-change="confirmChange($event)"
        />
    </div>
</template>

<script>
    import UserItem from "./UserItem";
    import NewUser from "./NewUser";
    import FilterUsers from "./FilterUsers";
    import HeaderMenu from "./HeaderMenu";
    import EditUser from "./EditUser";

    export default {
        name: "UserList",
        components: {
            HeaderMenu,
            FilterUsers,
            UserItem,
            NewUser,
            EditUser
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
                        creationDate: new Date(2018, 2, 15, 14, 14),
                        lastChangeDate: new Date(2018, 2, 15, 14, 21)
                    },
                    {
                        fullName: 'Борисов Борис Борисович',
                        email: 'bor256@fdfsd.ru',
                        password: '123',
                        userStatus: 'user',
                        telephone: 89497471515,
                        creationDate: new Date(2018, 2, 15, 14, 15),
                        lastChangeDate: new Date(2018, 2, 15, 14, 22)
                    },
                    {
                        fullName: 'Васильева Василиса Васильевна',
                        email: 'Vasilyeva1981@fdfsd.ru',
                        password: '123',
                        userStatus: 'client',
                        telephone: 89211112233,
                        creationDate: new Date(2018, 2, 15, 14, 16),
                        lastChangeDate: new Date(2018, 2, 15, 14, 23)
                    },
                    {
                        fullName: 'Геннадьев Геннадий Геннадьевич',
                        email: 'gena66@bgrt.ru',
                        password: '123',
                        userStatus: 'user',
                        telephone: 89772956710,
                        creationDate: new Date(2018, 2, 15, 14, 17),
                        lastChangeDate: new Date(2018, 2, 15, 14, 24)
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
            // Последовательность методов: editUser() из UserItem => editUser() в UserList =>
            // confirmChanges() в EditUser => confirmChange() в UserList
            editUser(userInfo) {
                const {user, id} = userInfo
                console.log('EditUser User object: ', user)
                console.log('EditUser User Id: ', id)
                this.isEditing = true
                this.sendingData = userInfo
            },
            // Принимает подтверждение для редактирования. Принимает эмит из EditUsers.
            confirmChange(newData) {
                const {info, userId} = newData
                this.$set(this.users, userId, info)
                this.users[userId].lastChangeDate = new Date(Date.now())
            },
            // Удаляет пользователя. Принимает эмит из компонента Users
            deleteUser(deletedUser) {
                const confirmation = confirm('Вы действительно хотите удалить этого пользователя?')
                confirmation === true ? this.users.splice(deletedUser, 1) : undefined
            },
            // Фильтр по email или телефону. Эмит из компонента FilterComponent
            filterData(info) {
                const {key, val} = info
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
