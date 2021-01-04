<template>
    <v-form class="mt-5">
        <h1 class="text-center">Редактирование пользователя ID: {{sendingData.id}}</h1>
        <v-text-field
                label="ФИО"
                :value="user.fullName"
                v-model="newInfo.fullName"
        ></v-text-field>
        <v-text-field
                label="Email"
                :value="user.email"
                v-model="newInfo.email"
        ></v-text-field>
        <v-text-field
                label="Пароль"
                :value="user.password"
                v-model="newInfo.password"
        ></v-text-field>
        <v-select
                label="Выберие статус пользователя"
                :items="userStatus"
                :value="user.userStatus"
                v-model="newInfo.userStatus"
        ></v-select>
        <v-text-field
                label="Телефон"
                :value="user.telephone"
                v-model="newInfo.telephone"
        ></v-text-field>
        <v-btn @click="confirmChanges">Подтвердить изменения</v-btn>
    </v-form>
</template>

<script>
    export default {
        name: "EditUser",
        created() {
            console.log(this.sendingData.fullName)
        },
        data() {
            return {
                userStatus: [
                    'user', 'client', 'admin'
                ],
                user: this.sendingData.user,
                newInfo: {
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
                const confirmation = confirm(`Вы действительно хотите изменить данные пользователя под номером ${this.sendingData.id}?`)
                if (confirmation === true) {
                    console.log('Edit user user: ', this.newInfo)
                    console.log('ID: ', this.sendingData.id)
                    const info = this.newInfo
                    const userId = this.sendingData.id
                    this.$emit('on-confirm-change', {info, userId})
                }
            },
        }
    }
</script>

<style scoped>

</style>
