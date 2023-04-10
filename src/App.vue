<template>
  <div id="app">
    <div v-for="category in Object.keys(list)" :key="category">
      {{ category }}
      <button @click="addCategory">+</button>
      <button @click="removeItem(item.id)">-</button>
      <ul>
        <li v-for="item in list[category].items" :key="item.id">
          <input type="text" v-model="item.text">
          <input type="number" v-model="item.num">
          <button @click="addItem">+</button>
          <button @click="removeItem(item.id)">-</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return ({
      current: 'hoge',
      list: {
        hoge: {
          items: [{
            id: 1,
            text: '',
            num: 0
          }]
        }
      }
    })
  },
  methods: {
    addItem() {
      this.list[this.current].items.push({
        id: this.list[this.current].items.length + 1,
        text: "",
        num: 0
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

li {
  list-style: square;
  text-align: left;
}
</style>
