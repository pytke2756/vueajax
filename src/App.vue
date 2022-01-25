<template>
  <div id="app">
    <nav>
      <router-link to="/">Paintings</router-link>
      <router-link to="/statues">Statues</router-link>
      <router-link to="/info">Info</router-link>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script>


export default {
  name: 'App',
  components: {
},
  data() {
    return {
        state:'Paintings'
      }
    },
}
</script>

<style>
  nav router-link{
    background-color:green ;
  }
</style>