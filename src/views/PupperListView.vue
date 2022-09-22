<template>
  <div class="home">
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
}
</script>