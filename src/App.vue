<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
    <div class="small-container">
      <h1>Employees</h1>
      <employee-form @add:employee="addEmployee"/>
      <employee-table 
      v-bind:employees ="employees" 
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
      />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import EmployeeTable from './components/EmployeeTable.vue'
import EmployeeForm from './components/EmployeeForm.vue';
export default {
  name: 'App',
  components: {
    HelloWorld,
    EmployeeTable,
    EmployeeForm
  },
  data(){
    return{
      employees:[
      // {
      //   id:1,
      //   name:'Thushanth Sbm',
      //   email: 'th@gmail.com'
      // },
      // {
      //   id:2,
      //   name:'Kanthan Stv',
      //   email: 'kat@gmail.com'
      // },
      // {
      //   id:3,
      //   name:'Jack',
      //   email: 'tty@gmail.com'
      // }
    ]
    }
  },
  methods:{
    async addEmployee(employee){
      try {

        const response = await fetch('https://jsonplaceholder.typicode.com/users',
          {
            method:'POST',
            body:JSON.stringify(employee),
            headers:{ 'Content-type': 'application/json; charset=UTF-8'}
          }
      );

      const data = await response.json();

      // const lastId = this.employees.length>0?this.employees[this.employees.length-1].id:0;
      // const id = lastId + 1;

      // const newEmployee = {
      //   ...employee,id:id
      // }
      
      // this.employees = [...this.employees,newEmployee];

      this.employees = [...this.employees,data];
      } catch (error) {
        console.log(error)
      }
      
    },
    async deleteEmployee(id){

      try {
            await fetch('https://jsonplaceholder.typicode.com/users/' +id,
                {
                  method:'DELETE'
                }
            );

        this.employees=this.employees.filter((employee)=>{
            return employee.id != id;
      })

      } catch (error) {
        console.log(error);
      }
      
    },
    async editEmployee(id,updatedEmployee){

      try {
        
              const response = await fetch('https://jsonplaceholder.typicode.com/users/' +id,
                {
                  method:'PUT',
                  body:JSON.stringify(updatedEmployee),
                  headers:{ 'Content-type': 'application/json; charset=UTF-8'}
                }
            );

            const data = await response.json();
            
            this.employees.map((employee)=>{
              // return employee.id==id?updatedEmployee:employee;
              return employee.id==id?data:employee;
            })

      } catch (error) {
        console.log(error);
      }
      
    },

    async getEmployees(){
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users' ,{method:'GET'})
        const data = await response.json();
        this.employees = data;

      } catch (error) {
        console.log(error)
      }

    }
  },
  mounted(){
    console.log('Calling Something !')
    this.getEmployees();
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
button{
background:#009435;
border:1px solid #009435;
}
.small-container{
   max-width:680px;
}
</style>
