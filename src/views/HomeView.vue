<template>
  <div class="home">
    <b-button variant="success" @click="addPupper()" class="m-2">Lisää pentu</b-button>
    <PupperList ref="pupperList" />
  </div>
</template>

<script>
import PupperList from '../components/PupperList.vue';

export default {
  name: 'HomeView',
  components: {
    PupperList
  },
  methods: {
    addPupper() {
      this.$router.push('/addpupper')
    }
  },
}
</script>