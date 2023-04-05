<template>
  <div class="container">
    <div class="card-header">
        <h4>
            Student
            <RouterLink to="/students/create" class="btn btn-primary float-end"> Add Student</RouterLink>
        </h4>
    </div>
    <div class="card-body">
        <table class="table table-bordered">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Course</th>
                    <th>Email</th>
                    <th>Phone</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(student,index) in this.students" :key="index">
                    <td>{{ student.id }}</td>
                    <td>{{ student.name }}</td>
                    <td>{{ student.course }}</td>
                    <td>{{ student.email }}</td>
                    <td>{{ student.phone }}</td>
                    <td>
                        <RouterLink :to="{ path: '/students/' +student.id+'/edit' }" class="btn btn-success"> Edit</RouterLink>
                        <button type="button" @click="deleteStudent(student.id)" class="btn btn-danger">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
  </div>
  </template>

  <script>

import  axios from 'axios'
  export default {
    name: 'Student',
    data() {
        return{
            students:[]
        }

    },
    mounted (){
        this.getStudents();
        //console.log('i am here...')
    },
    methods:{
        getStudents() {
        axios.get('http://127.0.0.1:8000/api/students').then( res => {
            this.students = res.data.students
            //console.log(this.students)

        });
    },
    deleteStudent(studentId){
        
        if(confirm('Are you sure you want to delete data?')){
            console.log(studentId)
            axios.delete(`http://127.0.0.1:8000/api/students/${studentId}/delete`).then(res =>{
                alert(res.data.message);
                this.getStudents();

            }).catch(function (error){
                if(error.response){
                    if (error.response. status == 404) {
                        alert(error.response.data.message);
                    }
                } 
            });
        }
    },
    },
  }
</script>