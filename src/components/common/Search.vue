<template>
  <div class="search">

  <input type="text" @keyup.enter="submtSearch" v-model="searchValue" :placeholder="msg">
  <button type="button" class="btn btn-success" @click="submtSearch">Submit Search</button>

  </div>
</template>

<script>
export default {
  name: 'Search',
  props: {
    msg: String
  },
  data () {
    return {
      searchValue: '',
    }
  },
  methods: {
    submtSearch() {
      this.$emit('onSubmit', this.searchValue);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  input {
    width: 500px;
  }
</style>
