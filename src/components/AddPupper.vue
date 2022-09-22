<template>
  <div class="form m-2">
    <b-form-group
      id="addPupperForm"
      label="Lisää pentu rekisteriin"
      label-for="input-1"
      valid-feedback="Thank you!"
      :invalid-feedback="isValid()"
      :state="state"
      label-class="mb-1"
    >
      <b-form-input class="m-2" v-model="pupper.id" placeholder="Rekisterinumero"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.name" placeholder="Nimi"></b-form-input>
      <div class="m-2">
        <label>Sukupuoli</label>
      </div>
      <div>
        <b-form-radio class="m-1" v-model="pupper.gender" name="some-radios" :value="true">Uros</b-form-radio>
        <b-form-radio class="m-1" v-model="pupper.gender" name="some-radios" :value="false">Naaras</b-form-radio>
      </div>
      <b-form-input class="m-2" v-model="pupper.astutus" placeholder="Astutus"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.jlpp" placeholder="JLPP"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.ulkomuodonTarkastus" placeholder="Ulkomuodon Tarkastus"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.koulutustunnus" placeholder="Koulutustunnus"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.luonnetesti" placeholder="Luonnetesti"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.mhLuonnekuvaus" placeholder="MH-luonnekuvaus"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.lonkat" placeholder="Lonkat"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.lonkatBlup" placeholder="Lonkat BLUP"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.kyynarnivel" placeholder="Kyynärnivel"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.silmat" placeholder="Silmät"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.selka" placeholder="Selkä"></b-form-input>
      <b-form-input class="m-2" v-model="pupper.kasvattaja" placeholder="Kasvattaja"></b-form-input>

      <b-button variant="outline-danger" @click="cancel()" class="me-1">Peruuta</b-button>
      <b-button variant="success" @click="addPupper()">Lisää pentu</b-button>
    </b-form-group>
  </div>
</template>

<script>
export default {
  name: 'addPupper',
  components: {
  },
  data() {
    return {
      search: "",
      pupper: {
        id: '',
        name: '',
        gender: true,
        bornAvailable: '',
        astutus: '',
        jlpp: '',
        ulkomuodonTarkastus: '',
        koulutustunnus: '',
        luonnetesti: '',
        mhLuonnekuvaus: '',
        lonkat: '',
        lonkatBlup: '',
        kyynarnivel: '',
        silmat: '',
        selka: '',
        kasvattaja: '',
      },
      valid: true
    };
  },
  computed: {
    invalidFeedback() {
      return 'Enter at least 4 characters.'
    }
  },
  methods: {
    isValid() {
      if (this.valid) return
      return "Täytä kaikki kentät"
    },
    async getEnvironment(branch){
      const contentful = require('contentful-management')
      const client = contentful.createClient({accessToken: process.env.VUE_APP_CTF_CMA_ACCESS_TOKEN})
      const space = await client.getSpace(process.env.VUE_APP_CTF_SPACE_ID)
      const environment = await space.getEnvironment(branch)
      return environment
    },

    cancel(){
      this.$router.push('/')
    },

    async addPupper() {
      this.valid = true;
      if (!this.pupper.id) {
        console.log('Pupper ID is required');
        this.valid = false;
        return;
      }
      if (!this.pupper.name) {
        this.valid = false;
        console.log('Pupper name is required');
        return;
      }
      console.log("CREATING NEW PUPPER...");

      const environment = await this.getEnvironment("master");
      const pupper = {
        fields: {
          id: {
            "en-US": this.pupper.id,
          },
          nimi: {
            "en-US": this.pupper.name,
          },
          sukupuoli: {
            "en-US": this.pupper.gender
          },
          bornAvailable: {
            "en-US": this.pupper.bornAvailable
          },
          jlpp: {
            "en-US": this.pupper.jlpp
          },
          ulkomuodonTarkastus: {
            "en-US": this.pupper.ulkomuodonTarkastus
          },
          koulutustunnus: {
            "en-US": this.pupper.koulutustunnus
          },
          luonnetesti: {
            "en-US": this.pupper.luonnetesti
          },
          mhLuonnekuvaus: {
            "en-US": this.pupper.mhLuonnekuvaus
          },
          lonkat: {
            "en-US": this.pupper.lonkat
          },
          lonkatBlup: {
            "en-US": this.pupper.lonkatBlup
          },
          kyynarnivel: {
            "en-US": this.pupper.kyynarnivel
          },
          selka: {
            "en-US": this.pupper.selka
          },
          kasvattaja: {
            "en-US": this.pupper.kasvattaja
          },
        }
      }
      let entry = await environment.createEntry('pentu', pupper)
      entry = await entry.publish();
      console.log("PUPPER CREATED");
      this.pupper = {
        id: '',
        name: '',
        gender: true,
      }
      this.$router.push('/')

    },
  },
}
</script>

<style scoped>
.form {
  display: block;
  text-align: left;
  margin: 0 auto;
  padding: 1em;
}

b-form-input {
  margin: 1em;
}
</style>