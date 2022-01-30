<template>
  <div class="container">
    <h1 class="title-auto-complete">Enter the name of your city</h1>
    <auto-complete :list-data="listData" @infoCity="infoCity" />
    <div v-if="checkData" class="show-info">
      <h1>Name city : {{ data.name }}</h1>
      <h2>Country : {{ data.sys.country }}</h2>
      <h3>cod : {{ data.cod }}</h3>
    </div>
  </div>
</template>

<script>
import AutoComplete from '~/components/AutoComplete.vue'
export default {
  name: 'IndexPage',
  components: { AutoComplete },
  data() {
    return {
      listData: ['London', 'Tehran', 'Mashhad', 'California'],
      data: {},
    }
  },
  computed: {
    checkData() {
      return Object.keys(this.data).length
    },
  },
  methods: {
    infoCity(data) {
      this.data = data
    },
  },
}
</script>
