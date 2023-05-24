<template>
  <div id="app">
    <div class="header">
      <div class="tab" :class="{active: current == title.id}" v-for="title in categories" :key="title.id" @click="load(title.id)" @dblclick="categoryNameEdit=true">
        <div v-if="categoryNameEdit" class="tab_input">
          <input type="text" v-model="title.name" @blur="categoryNameEdit=false">
        </div>
        <span>{{title.name}}</span>
        <button class="tabBtn removeBtn" @click="removeCategory(title.id)">×</button>
      </div>
      <button class="tabBtn appendTabBtn" @click="addCategory">+</button>
    </div>
    <ul>
      <li v-for="item in items" :key="item.id">
        <input type="checkbox" class="status" v-model="item.check">
        <input type="text" class="name" :class="{checked: item.check}" v-model="item.text">
        <!-- <input type="number" v-model="item.num"> -->
        <div class="toggle removeBtn"></div>
        <!-- <button class="removeBtn" @click="removeItem(item.id)">-</button> -->
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
      current: '',
      items:[],
      categories:[],
      categoryNameEdit: false
    })
  },
  methods: {
    load(id) {
      this.items = JSON.parse(localStorage.getItem(id))
      this.current = id
    },
    addItem() {
      this.items.push({
        id: crypto.randomUUID(),
        text: "taskName",
        num: 0,
        check: false
      })
      localStorage.setItem(this.current, JSON.stringify(this.items))
    },
    removeItem(id) {
      this.items = this.items.filter(item => item.id != id)
    },
    addCategory() {
      let nextName = "category" + this.categories.length
      let categoryId = crypto.randomUUID()
      this.categories.push ({
        id: categoryId,
        name: nextName
      })
      this.current = categoryId
      this.items=[{
          id: crypto.randomUUID(),
          text: 'taskName',
          num: 0,
          checked: false
      }]
    },
    removeCategory(id) {
      if (this.categoryNameEdit){
        this.categoryNameEdit = false
      } else {
        if (this.categories.length > 1) {
          this.categories = this.categories.filter(item => item.id != id)
        }
      }
    },
    confirmEdit (id, event){
      console.log(id)
      console.log(event)
      const target = this.categories.filter(item => item.id == id)
      target.name = event.target.value
    }
  },
  mounted() {
    const currentId = window.localStorage.getItem('current') || ''
    if (currentId || currentId.length) {
      this.categories = JSON.parse(localStorage.getItem('categories'))
      this.current = this.categories[0].id
      this.items = JSON.parse(localStorage.getItem(this.current))
    } else {
      this.categories.push({id: crypto.randomUUID(), name:'todo'})
      this.current = this.categories[0].id
      this.items.push({
          id: crypto.randomUUID(),
          text: "taskName",
          num: 0,
          check: false
      })
    }
  },
  watch: {
    current: function () {
        localStorage.setItem('current', this.current)
    },
    items: {
      handler: function (val) {
        localStorage.setItem(this.current, JSON.stringify(val))
      },
      deep: true
    },
    categories: {
      handler: function () {
        localStorage.setItem('categories', JSON.stringify(this.categories))
      },
      deep: true
    },
  },
}
</script>

<style>
@media (prefers-color-scheme: dark) {
  body {
    background-color: #202020;
    color: #736be2;
  }
  .tab {
    background-color: rgb(46, 43, 43);
  }
  .active { 
    background-color: lightgray;
  }
  .active > button{ 
   color: #202020;
  }
  .tab_input > button {
    color: #202020;
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

.toggle {
  display: inline-block;
  content:"";
	width: 6px;
	height: 6px;
	border-top: 2px solid #fff;
	border-right: 2px solid #fff;
	-webkit-transform: rotate(45deg);
	position: relative;
	top:calc( 50% - 3px );
	right: 20px;
	transform: rotate(135deg);
}
body {
  margin-top: -10px;
}
.header {
  margin-left: 10px;
  max-height: 45px;
  overflow-x: scroll;
  white-space: nowrap
}
.header::-webkit-scrollbar {
  display: none;
}
.tab {
  position: relative;
  padding-left: 10px;
  padding-right: 10px;
  font-size: 1.5rem;
  text-transform: uppercase;
  border: gray 1px solid;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  border-bottom: none;
  max-width: 250px;
  height: 44px;
  display: inline-block;
}
.tab>button{
  margin-left: 48px;
}

.tab_input{
  position: absolute;;
  background-color: inherit;
  margin-top: 5px;
  width: 120px;
}

.tab_input > input {
  width: 100%;
  border: 0px;
  font-size: 1.5rem;
  background-color: inherit;
  color: inherit;
  text-transform: uppercase;
}

.appendTabBtn {
  position: relative;
  bottom: 0px;
  left: 10px;
}

.tabBtn{
  background: transparent;
  border-style: none;
  color: lightGray;
  font-size: xx-large;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 20px;
  height: 500px;
}

ul{
  padding-left: 10px;
  margin-left: 0;
  margin-top: 0;
  list-style: none;
  border: gray 1px solid;
  border-radius: 10px;
  padding-bottom: 10px;
  padding-right: 10px;
}

li{
  position: relative;
  list-style: none;
  text-align: left;
  margin: 0px;
  padding: 10px 10px;
  border-bottom: 1px solid #666;
}

.name {
  font-size: 1.5rem;
  border: 0px;
  background-color: inherit;
  width: 90%;
}
input[type="checkbox"]{
  width: 20px;
  height: 24px;
  width: 5%;
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
.removeBtn {
  width: 30px;
  height: 30px;
}

</style>
