<template>
  <div class="home">
    <b-table-simple hover small caption-top responsive>
      <b-thead head-variant="dark">
        <b-tr>
          <b-th>Rekisterinumero</b-th>
          <b-th>Nimi</b-th>
          <b-th>Sukupuoli</b-th>
          <b-th>Astutus</b-th>
          <b-th>Syntynyt/Jäljellä</b-th>
          <b-th>JLPP</b-th>
          <b-th>Ulkomuodon tarkastus</b-th>
          <b-th>Koulutustunnus</b-th>
          <b-th>Luonnetesti</b-th>
          <b-th>MH-luonnekuvaus</b-th>
          <b-th>Lonkat</b-th>
          <b-th>Lonkat BLUP</b-th>
          <b-th>Kyynärnivel</b-th>
          <b-th>Silmät</b-th>
          <b-th>Selkä</b-th>
          <b-th>Kasvattaja</b-th>
        </b-tr>
      </b-thead>
      <b-tbody>
        <b-tr v-for="pupper in puppers" :key="pupper.sys.id">
          <b-td>{{pupper.fields.id}}</b-td>
          <b-td>{{pupper.fields.nimi}}</b-td>
          <b-td>{{pupper.fields.sukupuoli ? 'Uros':'Naaras'}}</b-td>
          <b-td>{{pupper.fields.bornAvailable}}</b-td>
          <b-td>{{pupper.fields.astutus}}</b-td>
          <b-td>{{pupper.fields.jlpp}}</b-td>
          <b-td>{{pupper.fields.ulkomuodonTarkastus}}</b-td>
          <b-td>{{pupper.fields.koulutustunnus}}</b-td>
          <b-td>{{pupper.fields.luonnetesti}}</b-td>
          <b-td>{{pupper.fields.mhLuonnekuvaus}}</b-td>
          <b-td>{{pupper.fields.lonkat}}</b-td>
          <b-td>{{pupper.fields.lonkatBlup}}</b-td>
          <b-td>{{pupper.fields.kyynarnivel}}</b-td>
          <b-td>{{pupper.fields.silmat}}</b-td>
          <b-td>{{pupper.fields.selka}}</b-td>
          <b-td>{{pupper.fields.kasvattaja}}</b-td>
        </b-tr>
      </b-tbody>
    </b-table-simple>
  </div>
</template>

<script>
import {createClient} from 'contentful';

const client = createClient({
  space: process.env.VUE_APP_CTF_SPACE_ID,
  accessToken: process.env.VUE_APP_CTF_CDA_ACCESS_TOKEN,
});

export default {
  name: 'HomeView',
  components: {
  },
  data() {
    return {
      puppers: [],
      search: "",
    };
  },
  mounted() {
    this.fetchpuppers();
  },
  methods: {
    async fetchpuppers() {
      console.log("FETCH");

      await client
        .getEntries({
          content_type: 'pentu',
          order: '-sys.createdAt',
          'fields.title[match]': this.search,
        })
        .then(puppers => this.puppers = puppers.items)
        .catch(err => console.log(err));
    },
  },
}
</script>

<style scoped>
table {
  margin: 0 auto;
}
</style>
