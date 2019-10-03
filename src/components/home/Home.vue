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
      entries: [],
      fields: [
        { key: "id", label: "Entrada", sortable: true },
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
      const url = `${baseApiUrl}/entries/${this.user.id}?page=${this.page}`;
      axios.get(url).then(res => {
        this.entries = this.entries.concat(res.data);

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
