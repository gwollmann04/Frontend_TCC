<template>
  <div class="home">
    <PageTitle icon="fa fa-home" main="Home" sub="Lista de Incidencias" />
    <template>
      <div>
        <b-table striped hover :items="entries" :fields="fields"></b-table> 
      </div>
    </template>
  </div>
</template>


<script>
import { baseApiUrl } from '@/global'
import axios from 'axios'
import { mapState } from "vuex";
import PageTitle from "../template/PageTitle";


export default {
  name: "Home",
  components: { PageTitle },
  computed: mapState(["user"]),
  data: function() {
    return {
      entries: [],
      fields: [
        { key: "id", label: "Entrada", sortable: true },
        { key: "typeof", label: "Tipo", sortable: true },
        { key: "createdat", label: "Data", sortable: true,
        formatter: (key) =>{
            var parts = key.split('T')
            return parts[0] + ' ' + parts[1]
        }}
      ]}
  },
  methods: {
    loadEntries() {
      const url = `${baseApiUrl}/entries/${this.user.id}`
      axios.get(url).then(res => {
      this.entries = res.data
      })
    }
  },
  mounted() {
    this.loadEntries();
  }
}

</script>

<style>
</style>
