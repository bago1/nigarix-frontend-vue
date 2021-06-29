<template>
  <div id="app">
        <Site/>
<!--    <AdminPanel/>-->
  </div>
</template>

<script>
import Site from './components/Site.vue'
import AdminPanel from './components/AdminPanel.vue'

export default {
  name: 'App',
  components: {
    Site,
    AdminPanel

  },
}
</script>

<style>

</style>
