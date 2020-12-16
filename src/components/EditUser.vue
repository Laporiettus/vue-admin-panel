<template>
    <v-form>
        <v-text-field
                label="Введите новые данные"
                v-model="newInfo"
        ></v-text-field>
        <v-btn @click="confirmChanges">Подтвердить изменения</v-btn>
    </v-form>
</template>

<script>
    export default {
        name: "EditUser",
        data() {
            return {
                newInfo: '',
                keyAlias: ''
            }
        },
        props: ['sendingData'],
        methods: {
            confirmChanges() {
                console.log(this.sendingData)
                this.aliasPicker(this.sendingData.ind)
                const confirmation = confirm(`Вы действительно хотите изменить ${this.keyAlias} пользователя?`)
                console.log(this.newInfo)
                if (confirmation === true) {
                    this.sendingData.val = this.newInfo
                    this.$emit('on-confirm-change', this.sendingData)
                } else {
                    return undefined
                }
            },
            aliasPicker(keyName) {
                switch (keyName) {
                    case 'fullName':
                        return this.keyAlias = 'ФИО'
                    case 'email':
                        return this.keyAlias = 'Email'
                    case 'password':
                        return this.keyAlias = 'пароль'
                    case 'userStatus':
                        return this.keyAlias = 'статус'
                    case 'telephone':
                        return this.keyAlias = 'номер телефона'
                }
            }
        }
    }
</script>

<style scoped>

</style>
