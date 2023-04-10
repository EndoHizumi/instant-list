<template>
  <div id="app">
    <h2 class="header">{{title}}</h2>
    <ul>
      <li v-for="item in items" :key="item.id">
        <input type="checkbox" v-model="item.check">
        <input type="text" class="name" :class="{checked: item.check}" v-model="item.text">
        <!-- <input type="number" v-model="item.num"> -->
        <button class="removeBtn" @click="removeItem(item.id)">-</button>
      </li>
      <button class="appendBtn" @click="addItem">+</button>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return ({
      title: 'default',
      items: [{
      "default":{
        id: 1,
        text: '',
        num: 0,
        check: false
      }}]
    })
  },
  methods: {
    addItem(){
      this.items.push({
        id: this.items.length +1,
        text:"",
        num:0,
        check: false
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
@media (prefers-color-scheme: dark) {
  body {
    background-color: #202020;
    color: #736be2;
  }
  li {
    background-color: #202020
  }
  .name{
    color: white;
  }
}

@media (prefers-color-scheme: light) {
  li {
    background-color: #d8d8d8;
  }
}

body {
  margin-top: -20px;
}
.header {
  margin-top: - 1.5rem;
  margin-right: - 1.5rem;
  margin-left: - 1.5rem;
  margin-bottom: 0px;
  padding:  .75rem;
  padding-bottom: 0px;
  font-size: 1.5rem;
  text-align: left;
  text-transform: uppercase;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 0px;
  height: 500px;
}
ul{
  padding-left: 10px;
  margin-left: 0;
  margin-top: 0;
  list-style: none;
  counter-reset: counter;
}
li{
  list-style: none;
  text-align: left;
  margin: 0px;
  padding: 10px;
  border-bottom: 1px solid #666;
}
.name {
  font-size: 1.5rem;
  border: 0px;
  width: 90%;
  background-color: inherit;
}
input:focus {
   outline:transparent 1px none;
}
.appendBtn{
  margin-top: 10px;
  width: 100%;
  height: 30px;
}
.checked {
  color: gray;
  text-decoration: line-through;
}
</style>
