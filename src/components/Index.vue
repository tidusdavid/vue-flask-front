<template>
  <div id="index">
    <ul>
      <li v-for="user in users" :key="user.name">Hello <b>{{ user.user }}</b> [{{ user.title }}]</li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Index',
  data: function() {
    return {
      users: []
    }
  },
  methods: {
    getUsers: function() {
      axios
        .get('http://localhost:5000/api/v1/consult')
        .then(response => {
          console.log('Response: ' + JSON.stringify(response.data))
          this.users = response.data
        })
        .catch(error => {
          console.error(error)
        })
    }
  },
  mounted: function() {
    this.getUsers()
  }
}
</script>