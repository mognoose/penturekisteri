<template>
  <div class="home">
    <AddPupper @added="added" />
  </div>
</template>

<script>
import AddPupper from '../components/AddPupper.vue';

export default {
  name: 'HomeView',
  components: {
    AddPupper,
  },
}
</script>