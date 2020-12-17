<template>
    <v-form
            ref="form"
            class="form-component"
            lazy-validation
    >
        <v-text-field
                v-model="user.name"
                :counter="60"
                label="ФИО"
                outlined
                required
        ></v-text-field>

        <v-text-field
                v-model.trim="user.email"
                label="E-mail"
                outlined
                required
        ></v-text-field>

        <v-text-field
                v-model.trim="user.telephone"
                :counter="10"
                label="Номер телефона"
                outlined
                required
        ></v-text-field>

        <v-text-field
                v-model.trim="user.password"
                label="Пароль"
                outlined
                required
        ></v-text-field>

        <v-select
                v-model="user.status"
                :items="user.userStatus"
                label="Статус аккаунта пользователя"
                outlined
                required
        ></v-select>

        <v-btn
                color="success"
                class="mr-4"
                @click="createUser"
        >
            Validate
        </v-btn>
    </v-form>
</template>

<script>
    export default {
        name: "NewUser",
        data() {
            return {
                user: {
                    name: '',
                    email: '',
                    telephone: 8,
                    status: '',
                    userStatus: [
                        'User', 'Client', 'Admin'
                    ],
                    password: '',
                    creationDate: Date.now(),
                    lastChangedAt: Date.now()
                }
            }
        },
        methods: {
            createUser() {
                this.$emit(
                    'on-new-user', this.user
                )
            }
        }
    }
</script>

<style scoped>
    .form-component {
        margin-top: 40px;
    }
</style>
