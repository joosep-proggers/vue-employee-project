<script>
import EmployeeDefault from './components/EmployeeDefault.vue'
import DetailViewModal from './components/Modal.vue';
import axios from "axios"

  export default {
    data () {
      return {
        employees: null,
        endpoint1: 'https://tallinn-jobapp.aw.ee/employee_list/',
        endpoint2: 'https://tartu-jobapp.aw.ee/employee_list/',
        isModalVisible: false,
        selectedEmployee: null,
        positions: null,
    }
  },

    mounted() {
      this.getAllEmployees()
    },

    components: {
      EmployeeDefault, DetailViewModal
    },

    methods: {
      async getAllEmployees() {
        let employees1 = await axios.get(this.endpoint1)
          .then (function(response){
            let employees = response.data.employees
            return employees
          })
          .catch(error => {
            console.log(error)
            alert("An error ocurred" + error)
          })

        let employees2 = await axios.get(this.endpoint2)
          .then (function(response){
            let employees = response.data.employees
            return employees
          })
          .catch(error => {
            console.log(error)
            alert("An error ocurred" + error)
          })

        this.employees = [...employees1,...employees2]
        this.employees.sort((a, b) => a.lname.localeCompare(b.lname));
        this.positions = [...new Set(this.employees.map((item) => item.position))]
        this.positions.sort((a,b) => a.localeCompare(b))
        /*this.employees = this.employees.filter((value, index, self) =>
          index === self.findIndex((t) => (
            t.place === value.place && t.name === value.name
          ))
        )*/
    },
    showModal(employee) {
      this.selectedEmployee = employee
      document.documentElement.style.overflow = 'hidden'
      this.isModalVisible = true;
    },
    closeModal() {
      this.selectedEmployee = null;
      document.documentElement.style.overflow = 'auto'
      this.isModalVisible = false;
    }
  }
}
</script>

<template>
  <div id="app">
    <div v-for="position in positions" v-bind:key="position">
      <h1> {{ position }} </h1>
      <div v-for="employee in employees" v-bind:key="employee.fname">
        <EmployeeDefault @click="showModal(employee)" v-if="employee.position == position" :fname="employee.fname" :lname="employee.lname"/>
        <DetailViewModal v-if="selectedEmployee" :employeeData="selectedEmployee" v-show="isModalVisible" @close="closeModal"/>
      </div>
    </div>
  </div>
</template>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
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
</style>
