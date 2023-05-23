<template>
  <div id="app">
    <div class="header">
      <div class="tab" :class="{active: current == title}" v-for="title in Object.keys(list)" :key="title" @click="current = title" @dblclick="categoryNameEdit=true">
        <div v-if="categoryNameEdit" class="tab_input">
          <input type="text" :value="title">
        </div>
        <span :title="title">{{title}}</span>
        <button class="tabBtn removeBtn" @click="removeCategory(title)">×</button>
      </div>
      <button class="tabBtn appendTabBtn" @click="addCategory">+</button>
    </div>
    <ul>
      <li v-for="item in list[current].items" :key="item.id">
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
      current: 'Todo',
      list: {
        "Todo": {
          items: []
        }
      },
      categoryNameEdit: false
    })
  },
  methods: {
    addItem() {
      this.list[this.current].items.push({
        id: this.list[this.current].items.length + 1,
        text: "taskName",
        num: 0,
        check: false
      })
    },
    removeItem(id) {
      this.list[this.current].items = this.list[this.current].items.filter(item => item.id != id)
    },
    addCategory() {
      let nextName = "category" + Object.keys(this.list).length
      this.$set(this.list, nextName, {
        items: [{
          id: 1,
          text: 'taskName',
          num: 0,
          checked: false
        }]
      })
    },
    removeCategory(name) {
      if (this.categoryNameEdit){
        this.categoryNameEdit = false
      } else {
        this.$delete(this.list, name)
        this.current = Object.keys(this.list)[Object.keys(this.list).length - 1]
      }
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
    this.current = Object.keys(this.list)[0]}
  }
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
