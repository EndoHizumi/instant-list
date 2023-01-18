<template>
  <div id="app">
    <ul>
      <li v-for="item in items" :key="item.id">
        <input type="text" v-model="item.text">
        <input type="number" v-model="item.num">
        <button @click="addItem">+</button>
        <button @click="removeItem(item.id)">-</button>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return ({
      items: [{
        id: 1,
        text: '',
        num: 0
      }]
    })
  },
  methods: {
    addItem(){
      this.items.push({
        id: this.items.length +1,
        text:"",
        num:0
      })
    },
    removeItem(id) {
      this.items = this.items.filter(item => item.id != id)
    }
  },
  watch: {
    items: {
      handler: function (val) {
          window.localStorage.setItem('instantList',JSON.stringify((val)))
        },
        deep: true
    }
  },
  mounted () {
    if (window.localStorage.getItem('instantList')) {
      this.items = JSON.parse(localStorage.getItem("instantList"))
    }
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
li{
  list-style: square;
  text-align: left;
}
</style>
