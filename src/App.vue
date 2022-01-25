<template>
  <div id="app">
    <router-link to="/">Paintings</router-link>
    <router-link to="/statues">Statues</router-link>
    <router-link to="/info">Info</router-link>
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