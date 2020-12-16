<template>
    <v-form
            ref="form"
            class="form-component"
            lazy-validation
            v-model="formValidation"
    >
        <v-text-field
                v-model="user.name"
                :rules="rules.nameValidation"
                :counter="60"
                label="ФИО"
                outlined
                required
        ></v-text-field>

        <v-text-field
                v-model.trim="user.email"
                label="E-mail"
                :rules="rules.emailValidation"
                outlined
                required
        ></v-text-field>

        <v-text-field
                v-model.trim="user.telephone"
                :rules="rules.telephoneValidation"
                :counter="10"
                label="Номер телефона"
                outlined
                required
                type="telephone"
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
                :disabled="!formValidation"
        >
            Создать пользователя
        </v-btn>
    </v-form>
</template>

<script>
    export default {
        name: "NewUser",
        data() {
            return {
                formValidation: false,
                user: {
                    name: '',
                    email: '',
                    telephone: 8,
                    status: '',
                    userStatus: [
                        'user', 'client', 'admin'
                    ],
                    password: '',
                    createdAt: new Date(Date.now()),
                    lastChangedAt: new Date(Date.now()),
                },
                rules: {
                    nameValidation: [
                        v => !!v || 'Поле ФИО должно быть заполнено',
                        v => /[a-zA-Zа-яА-Я]+/.test(v) || 'Введите корректные ФИО'
                    ],
                    emailValidation: [
                        v => !!v || 'Поле Email должно быть заполнено',
                        v => /.+@.+\..+/.test(v) || 'Введите корректный Email',
                    ],
                    telephoneValidation: [
                        v => !!v || 'Поле Телефон должно быть заполнено',
                        v => /^[0-9]{10}$/.test(v) || 'Введите корректный номер телефона',
                    ]
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
