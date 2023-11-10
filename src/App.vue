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
        errorMessageIsVisible: false,
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
        this.errorMessageIsVisible = false
        this.employees = null
        this.positions = null
        let employees1 = await axios.get(this.endpoint1)
          .then (function(response){
            let employees = response.data.employees
            return employees
          })
          .catch(error => {
            console.log(error)
            return
          })

        let employees2 = await axios.get(this.endpoint2)
          .then (function(response){
            let employees = response.data.employees
            return employees
          })
          .catch(error => {
            console.log(error)
            return
          })

        if (employees1 == undefined && employees2 == undefined){
          this.errorMessageIsVisible = true
        } else {
          this.employees = [...employees1,...employees2]
          this.employees = this.removeDuplicatesByProperties(this.employees, 'fname', 'lname')
          this.employees.sort((a, b) => a.lname.localeCompare(b.lname));
          this.positions = [...new Set(this.employees.map((item) => item.position))]
          this.positions.sort((a,b) => a.localeCompare(b))
        }
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
      },
      removeDuplicatesByProperties(arr, property1, property2) {
        const uniqueObjects = [];
        const uniqueValues = new Set();

        for (let obj of arr) {
          let value = obj[property1] + obj[property2];

          if (!uniqueValues.has(value)) {
            uniqueValues.add(value);
            uniqueObjects.push(obj);
          }
        }

        return uniqueObjects;
      }
    }
  }
</script>

<template>
  <div class="refresh-button">
    <button @click="getAllEmployees()" class="refresh-button-self" type="button">Refresh data</button>
  </div> 
  <div v-if="errorMessageIsVisible">
    <h1> An error ocurred trying to fetch data </h1>
  </div>
  <div class="employees">
    <div class="position" v-for="position in positions" v-bind:key="position">
      <h1> {{ position }} </h1>
      <div v-for="employee in employees" v-bind:key="employee.fname">
        <EmployeeDefault @click="showModal(employee)" v-if="employee.position == position" :fname="employee.fname" :lname="employee.lname"/>
      </div>
    </div>
  </div>
  <div id="detail-view-modal">
    <DetailViewModal v-if="selectedEmployee" :employeeData="selectedEmployee" v-show="isModalVisible" @close="closeModal"/>
  </div>
  <div class="loading-icon" v-if="employees == null && errorMessageIsVisible == false">
    <font-awesome-icon :icon="['fas', 'spinner']" spin size="6x" />
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

.position {
  width: 30vw;
  margin-bottom: 5vh;
}

.employees {
   display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.refresh-button {
  display:flex;
  justify-content: center;
  
}

.refresh-button-self {
  background-color: #FAFBFC;
  border: 1px solid rgba(27, 31, 35, 0.15);
  border-radius: 6px;
  box-shadow: rgba(27, 31, 35, 0.04) 0 1px 0, rgba(255, 255, 255, 0.25) 0 1px 0 inset;
  box-sizing: border-box;
  color: #24292E;
  cursor: pointer;
  display: inline-block;
  font-family: -apple-system, system-ui, "Segoe UI", Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji";
  font-size: 14px;
  font-weight: 500;
  line-height: 20px;
  list-style: none;
  padding: 6px 16px;
  position: relative;
  transition: background-color 0.2s cubic-bezier(0.3, 0, 0.5, 1);
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  vertical-align: middle;
  white-space: nowrap;
  word-wrap: break-word;
}

.loading-icon {
  margin-top: 30vh;
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
