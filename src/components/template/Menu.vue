<template>
  <aside class="menu" v-show="isMenuVisible">
    <span>Nome: {{user.name}}</span>
    <span>E-mail: {{user.email}}</span>
    <span>Numero de Telefone:{{user.phoneNumber}}</span>
    <b-button variant="warning" @click.prevent="config">
      <i class="fa fa-pencil"></i>
    </b-button>
    <b-button variant="danger" @click.prevent="remove">
      <i class="fa fa-trash"></i>
    </b-button>
    <b-button variant="info" @click.prevent="logout">
      <i class="fa fa-sign-out"></i>
    </b-button>
  </aside>
</template>

<script>
import { baseApiUrl, showError } from '@/global'
import axios from 'axios'
import { userKey } from '@/global'
import { mapState } from "vuex";

export default {
  name: "Menu",
  computed: mapState(["isMenuVisible", "user"]),
  methods: {
        logout() {
            localStorage.removeItem(userKey)
            this.$store.commit('setUser', null)
            this.$router.push({ name: 'auth'})
        },
        config() {
            this.$router.push({ name: 'adminPages'})
        },
        remove() {
            axios.delete(`${baseApiUrl}/users/${this.user.id}`)
                .then(() => {
                    localStorage.removeItem(userKey)
                    this.$store.commit('setUser', null)
                    this.$router.push({ name: 'auth'})
                    this.$toasted.global.defaultSuccess()
                })
                .catch(showError)
        }
    }
}
</script>

<style>
.menu {
  grid-area: menu;
  background: linear-gradient(to right, #232526, #414345);

  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
}
span {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  color: rgb(192, 144, 144);
}
.btn{
    display: flex;
    margin-top: 20px;
    margin-right: 120px;
    margin-left: 120px;
}
</style>