<template>
  <div class="home">
    <ModalStudents/>
  </div>
</template>

<script>
// @ is an alias to /src
import ModalStudents from '@/components/ModalStudents.vue';

export default {
  name: 'HomeView',
  components: {
    ModalStudents,
  },
  mounted() {
    console.log(this.$route.params.id);
  },
}
</script>
