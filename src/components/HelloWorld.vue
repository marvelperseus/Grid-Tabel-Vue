<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <form id="search">
      Search <input name="query" v-model="searchQuery">
    </form>
    <table>
    <thead>
      <tr>
        <th v-for="key in columns" @click="sortBy(key)" :class="{ active: sortKey == key }">
          {{ key | capitalize }}
          <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'dsc'">
          </span>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="filteredData in filteredDatas">
        <td v-for="entry in filteredData">{{entry}}</td>
      </tr>
    </tbody>
    </table>
    <div>
      <div class="inputtext">
        <span>Name</span> <InputText v-model="newNameText" placeholder="New Name"/>
      </div>
      <div class="inputtext">
        <span>Power</span> <InputText	v-model="newPowerText" placeholder="New Power"/>
      </div>
      <button id="addButton" @click="addTodo">Add</button>
    </div>
  </div>
</template>

<script>
import InputText from './InputText.vue'

export default {
  name: 'HelloWorld',
  components: {
    InputText
  },
  props: {
    msg: String,
    name:{
      type: String,
      default:""
    }
  },
  computed: {
    filteredDatas: function () {
      var sortKey = this.sortKey

    var filterKey = this.searchQuery && this.searchQuery.toLowerCase()
      var order = this.sortOrders[sortKey] || 1
      var data = this.data
      if (filterKey) {
        data = data.filter(function (row) {
          return Object.keys(row).some(function (key) {
            return String(row[key]).toLowerCase().indexOf(filterKey) > -1
          })
        })
      }
      if (sortKey) {
        data = data.slice().sort(function (a, b) {
          a = a[sortKey]
          b = b[sortKey]
          return (a === b ? 0 : a > b ? 1 : -1) * order
        })
      }
      return data
    }
  },
  methods: {
    addTodo () {
      const trimmedTextName = this.newNameText.trim()
      const trimmedTextPower = this.newPowerText.trim()
			if (trimmedTextName) {
				this.data.push({
          name: trimmedTextName,
          power: trimmedTextPower
				})
        this.newNameText = ''
        this.newPowerText = ''
			}
		},
    sortBy: function (key) {
      this.sortKey = key
      this.sortOrders[key] = this.sortOrders[key] * -1
    }
  },
  filters: {
    capitalize: function (str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  },
  data(){
    var sortOrders = {}
    sortOrders['name'] = 1
    sortOrders['power'] = 1
   
    return {
      sortKey: '',
      newNameText:'',
      newPowerText:'',
      searchQuery: '',
      sortOrders: sortOrders,
      columns: ['name', 'power'],
      data: [
      { name: 'Chuck Norris', power: Infinity },
      { name: 'Bruce Lee', power: 9000 },
      { name: 'Jackie Chan', power: 7000 },
      { name: 'Jet Li', power: 8000 }
    ]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
  border: 2px solid #42b983;
  border-radius: 3px;
  background-color: #fff;
  margin: 20px auto;
}

th {
  background-color: #42b983;
  color: rgba(255,255,255,0.66);
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

td {
  background-color: #f9f9f9;
}

th, td {
  min-width: 120px;
  padding: 10px 20px;
}

th.active {
  color: #fff;
}

th.active .arrow {
  opacity: 1;
}

.arrow {
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.66;
}

.arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid #fff;
}

.arrow.dsc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid #fff;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.inputtext {
  display: -webkit-inline-box;
}
</style>
