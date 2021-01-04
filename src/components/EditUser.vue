<template>
    <v-form class="mt-5">
        <h1 class="text-center">Редактирование пользователя ID: {{sendingData.id}}</h1>
        <v-text-field
                label="ФИО"
                v-model="newUserInfo.fullName"
        ></v-text-field>
        <v-text-field
                label="Email"
                v-model="newUserInfo.email"
        ></v-text-field>
        <v-text-field
                label="Пароль"
                v-model="newUserInfo.password"
        ></v-text-field>
        <v-select
                label="Выберие статус пользователя"
                :items="userStatus"
                v-model="newUserInfo.userStatus"
        ></v-select>
        <v-text-field
                label="Телефон"
                v-model="newUserInfo.telephone"
        ></v-text-field>
        <v-btn @click="confirmChanges">Подтвердить изменения</v-btn>
    </v-form>
</template>

<script>
    export default {
        name: "EditUser",
        data() {
            return {
                userStatus: [
                    'user', 'client', 'admin'
                ],
                user: this.sendingData.user,
                newUserInfo: {
                    fullName: this.sendingData.user.fullName,
                    email: this.sendingData.user.email,
                    password: this.sendingData.user.password,
                    userStatus: this.sendingData.user.userStatus,
                    telephone: this.sendingData.user.telephone,
                    creationDate: this.sendingData.user.creationDate,
                    lastChangeDate: this.sendingData.user.lastChangeDate
                }
            }
        },
        props: {
            sendingData: {
                type: Object,
                props: {
                    id: {
                        type: Number
                    },
                    user: {
                        type: Object,
                        props: {
                            fullName: {
                                type: String
                            },
                            email: {
                                type: String
                            },
                            password: {
                                type: String
                            },
                            userStatus: {
                                type: String
                            },
                            telephone: {
                                type: Number,
                                maxLength: 10
                            },
                            creationDate: {
                                type: String
                            },
                            lastChangeDate: {
                                type: String
                            }
                        }
                    }
                }
            }
        },
        methods: {
            confirmChanges() {
                const confirmation = confirm(
                    `Вы действительно хотите изменить данные пользователя под номером ${this.sendingData.id}?`
                )
                if (confirmation === true) {
                    const info = this.newUserInfo
                    const userId = this.sendingData.id
                    this.$emit('on-confirm-change', {info, userId})
                }
            },
        }
    }
</script>

<style scoped>

</style>
