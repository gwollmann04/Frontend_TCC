<template>
    <div class="user-admin">
        <b-form>
            <input id="user-id" type="hidden" v-model="user.id" />
            <b-row>
                <b-col md="6" sm="12">
                    <b-form-group label="Nome:" label-for="user-name">
                        <b-form-input id="user-name" type="text"
                            v-model="user.name" required
                            placeholder="Informe o Nome do Usuário..." />
                    </b-form-group>
                </b-col>
                <b-col md="6" sm="12">
                    <b-form-group label="E-mail:" label-for="user-email">
                        <b-form-input id="user-email" type="text"
                            v-model="user.email" required
                            placeholder="Informe o E-mail do Usuário..." />
                    </b-form-group>
                </b-col>
            </b-row>
            <b-row>
                <b-col md="6" sm="12">
                    <b-form-group label="Senha:" label-for="user-password">
                        <b-form-input id="user-password" type="password"
                            v-model="user.password" required
                            placeholder="Informe a Senha do Usuário..." />
                    </b-form-group>
                </b-col>
                <b-col md="6" sm="12">
                    <b-form-group label="Confirmação de Senha:" 
                        label-for="user-confirm-password">
                        <b-form-input id="user-confirm-password" type="password"
                            v-model="user.confirmPassword" required
                            placeholder="Confirme a Senha do Usuário..." />
                    </b-form-group>
                </b-col>
            </b-row>
            <b-row>
                <b-col md="6" sm="12">
                    <b-form-group label="Numero de Telefone" 
                        label-for="user-phoneNumber">
                        <b-form-input id="user-phoneNumber" type="text"
                            v-model="user.phoneNumber"
                            placeholder="Informe o numero de telefone..." />
                    </b-form-group>
                </b-col>
            </b-row>
            <b-row>
                <b-col xs="12">
                    <b-button variant="primary" 
                        @click="save">Salvar</b-button>
                </b-col>
            </b-row>
        </b-form>
        <hr>
    </div>
</template>

<script>
import { baseApiUrl, showError } from '@/global'
import axios from 'axios'
import { mapState } from "vuex";

export default {
    name: 'UserAdmin',
    computed: mapState(["user"]),
    methods: {
        save() {
            const sendUser = {
                name : this.user.name,
                email: this.user.email,
                password: this.user.password,
                confirmPassword: this.user.confirmPassword,
                phoneNumber: this.user.phoneNumber
            }
            axios.put(`${baseApiUrl}/users/${this.user.id}`, sendUser)
                .then(() => {
                    this.$toasted.global.defaultSuccess()
                })
                .catch(showError)
        }
    }
}
</script>

<style>

</style>
