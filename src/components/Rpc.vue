<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    {{ resp }}
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      resp: ''
    }
  },
  created () {
    axios.post('http://10.0.42.106:3030', {
      'jsonrpc': '2.0',
      'method': 'say_hello',
      'params': {test: 42, more: [23, 16]},
      'id': 1
    })
    .then((resp) => {
      console.log(resp)
      this.resp = resp.data
    })
    .catch((err) => {
      console.log(err)
    })
  },
  mounted () {

  },
  methods: {

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

</style>
