<template>
  <div>
    <table>
      <thead>
        <tr>
          <th>Azonsító</th>
          <th>Person</th>
          <th>Height</th>
          <th>Price</th>
          <th>Műveletek</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="statue in statues" v-bind:key="statue.id">
          <td>{{ statue.id }}</td>
          <td><router-link :to="'/statues/'+statue.id">{{ statue.person }}</router-link></td>
          <td>{{ statue.height }}</td>
          <td>{{ statue.price }}</td>
          <td>
            <button @click="deleteStatue(statue.id)">Törlés</button>
            <button @click="editStatue(statue.id)">Szerkesztés</button>
          </td>
        </tr>
        <tr>
          <td>
            <input type="hidden" v-model="statue.id">
          </td>
          <td>
            <input type="text" v-model="statue.person">
          </td>
          <td>
            <input type="number" v-model="statue.height">
          </td>
          <td>
            <input type="number" v-model="statue.price">
          </td>
          <td>
            <button v-if="mod_new_statue" @click="newStatue" :disabled="statueSaving">Létrehoz</button>
            <button v-if="!mod_new_statue" @click="saveStatue" :disabled="statueSaving">Mentés</button>
            <button v-if="!mod_new_statue" @click="cancelEdit" :disabled="statueSaving">Mégse</button>
          </td>
        </tr>
      </tbody>
    </table>
    <button @click="loadStatues">Szobrok betöltése</button>
  </div>
</template>

<script>

export default {
  name: 'Statues',
  components: {
  },
  data() {
    return {
      mod_new_statue: true,
      statueSaving: false,
      statue: {
        id: null,
        person: '',
        height: '',
        price: ''
      },
      statues: [],
    }
  },
  methods: {
    async loadStatues(){
      let Response = await fetch('http://127.0.0.1:8000/api/statues')
      let data = await Response.json()
      this.statues = data
    },

    async deleteStatue(id){
        let Response = await fetch(`http://127.0.0.1:8000/api/statues/${id}`, {
        method: 'DELETE'
      })
      console.log(Response)
      await this.loadStatues()
    },
    async newStatue(){
      this.statueSaving='disabled'
     await fetch('http://127.0.0.1:8000/api/statues', {
       method: 'POST',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.statue) 
     })
     await this.loadStatues()
     this.statueSaving=false
     this.resetStatueForm()
    },
    async saveStatue(){
        this.statueSaving='disabled'
        await fetch(`http://127.0.0.1:8000/api/statues/${this.statue.id}`, {
       method: 'PATCH',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.statue) 
     })
     await this.loadStatues()
     this.statueSaving=false
     this.resetStatueForm()
    },
    async editStatue(id){
      let Response = await fetch(`http://127.0.0.1:8000/api/statues/${id}`)
      let data = await Response.json()
      this.statue = {...data};
      this.mod_new_statue = false
    },

    cancelEdit () {
      this.resetForm()
    },
    resetStatueForm() {
      this.statue = {
        id: null,
        person: '',
        height: '',
        price: ''
      }
      this.mod_new_statue = true
    },

  },
  mounted() {
    this.loadData(),
    this.loadStatues()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
