<template>
<div>
<ul>
        <li>{{ this.statue.id }}</li>
        <li>{{ this.statue.person }}</li>
        <li>{{ this.statue.height }}</li>
        <li>{{ this.statue.price }}</li>
    </ul>
</div>

</template>
<script>
    export default{
        name: 'Statues',
        components: {
        },
        data() {
        return {
            statue:[],
        }
    },
  methods: {
    async loadStatue(){
        const id = this.$route.params.id
      let Response = await fetch(`http://127.0.0.1:8000/api/statues/${id}`)
      let data = await Response.json()
      this.statue = {...data}
    },
  },
    mounted(){
      this.loadStatue()
    }
}

</script>
    
<style>
</style>
