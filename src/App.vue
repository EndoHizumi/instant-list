<template>
  <div id="app">
    <div v-for="category in Object.keys(list)" :key="category"></div>
      <h2 class="header">{{title}}</h2>
      <ul>
        <li v-for="item in list[category].iitems" :key="item.id">
          <input type="checkbox" v-model="item.check">
          <input type="text" class="name" :class="{checked: item.check}" v-model="item.text">
          <!-- <input type="number" v-model="item.num"> -->
          <button class="removeBtn" @click="removeItem(item.id)">-</button>
        </li>
        <button class="appendBtn" @click="addItem">+</button>
      </ul>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return ({
      title: 'default',
      current: 'hoge',
      list: {
        hoge: {
          items: [{
      "default":{
            id: 1,
            text: '',
            num: 0,
        check: false
          }}]
        }
      }
    })
  },
  methods: {
    addItem() {
      this.list[this.current].items.push({
        id: this.list[this.current].items.length + 1,
        text: "",
        num: 0,
        check: false
      })
    },
    removeItem(id) {
      this.list[this.current].items = this.list[this.current].items.filter(item => item.id != id)
    },
    addCategory() {
      let nextName = "category" + Object.keys(this.list).length
      console.log(nextName)
      this.list = {[nextName]: {
        items: [{
          id: 1,
          text: '',
          num: 0
        }]
      }
    }
      console.dir(this.list)
  },
  removeCategory() {

  }
},
watch: {
  list: {
    handler: function (val) {
      window.localStorage.setItem('instantList', JSON.stringify((val)))
    },
    deep: true
  }
},
mounted() {
  const instantList = window.localStorage.getItem('instantList') || ''
  if (instantList || instantList.length) {
    this.list = JSON.parse(localStorage.getItem("instantList"))
    this.current = Object.keys(this.list)[0]
  } else {
    this.list = {
      hoge: {
        items: [{
          id: 1,
          text: '',
          num: 0
        }]
      }
    }
    this.current = 'hoge'
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
