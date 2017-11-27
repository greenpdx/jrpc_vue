<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="clk_h">Hello</button>
    <button @click="clk_b">Bye</button>
    <button @click="clk_i">Ins</button>
    <button @click="clk_q">Qry</button>
    <button @click="clk_m">More</button><br>
    {{ dsply }}
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      resp: '',
      dsply: ''
    }
  },
  created () {
    this.id = 1
    this.clk_h('')
  },
  mounted () {

  },
  methods: {
    sendRpc (cmd, params, cb) {
      axios.post('http://10.0.42.106:3030', {
        'jsonrpc': '2.0',
        'method': cmd,
        'params': params,
        'id': this.id++
      })
      .then((resp) => {
        let data = resp.data
        console.log('RAWRESP', data, resp)
        cb(data)
      })
      .catch((err) => {
        console.log(err)
        cb(null)
      })
    },
    clk_h (evt) {
      this.sendRpc('say_hello', [42, 23], (full) => {
        console.log('Hello', full)
        this.resp = full.result
      })
    },
    clk_b (evt) {
      this.sendRpc('say_bye', [42, 23], (full) => {
        console.log('Bye', full)
        this.resp = full
      })
    },
    clk_i (evt) {
      this.sendRpc('say_ins', [42, 23], (full) => {
        console.log('Ins', full)
        this.resp = full
      })
    },
    clk_q (evt) {
      this.sendRpc('say_qry', [42, 23], (full) => {
        let resp = JSON.parse(full.result)
        console.log('Qry', resp.length, typeof resp, resp)
        this.resp = resp
      })
    },
    clk_m (evt) {
      this.sendRpc('say_more', [42, 23], (full) => {
        console.log('More', full)
        this.resp = full
      })
    }
  },
  watch: {
    resp: function (val) {
      this.dsply = val
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

</style>
