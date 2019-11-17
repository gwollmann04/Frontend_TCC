<template>
  <div class="home">
    <PageTitle icon="fa fa-home" main="Home" sub="Lista de Incidencias" />
    <template>
      <div>
        <b-table striped hover :items="entries" :fields="fields"></b-table>
      </div>
    </template>
    <div class="load-more">
      <button
        v-if="loadMore"
        class="btn btn-lg btn-outline-primary"
        @click="loadEntries"
      >Carregar Mais Entradas</button>
      <span v-else>Voce não possui novas entradas.</span>
    </div>
    <br>
    <template>
      <div>
        <b-table striped hover :items="values" :fields="types"></b-table>
      </div>
    </template>
  </div>
</template>


<script>
import { baseApiUrl } from "@/global";
import axios from "axios";
import { mapState } from "vuex";
import PageTitle from "../template/PageTitle";

export default {
  name: "Home",
  components: { PageTitle },
  computed: mapState(["user"]),
  data: function() {
    return {
      page: 1,
      loadMore: true,
      types: [
        {key:"typeof", label: "Tipo"},
        {key:"db", label: "Valor em Db"},
        ],
      values: [{typeof:"Alerta 1",db:"De 80 a 90 DB's"},
      {typeof:"Alerta 2",db:"De 90 a 100 DB's"},
      {typeof:"Alerta 3",db:"100 ou mais DB's"}],
      entries: [],
      fields: [
        { key: "id", label: "Entrada", sortable: true},
        { key: "typeof", label: "Tipo", sortable: true },
        {key: "createdat",label: "Data",sortable: true,
          formatter: key => {
            var parts = key.split("T");
            return parts[0];
          }
        }
      ]
    };
  },
  methods: {
    loadEntries() {
      var entryCount = 1;
      const url = `${baseApiUrl}/entries/${this.user.id}?page=${this.page}`;
      axios.get(url).then(res => {
        this.entries = this.entries.concat(res.data);

       
         this.entries.forEach(element => {
           element.id = entryCount++;
         });

        this.page++;
        if (res.data.length === 0) this.loadMore = false;
      });
    }
  },
  mounted() {
    this.loadEntries();
  }
};
</script>

<style>
.load-more {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 25px;
}
</style>
