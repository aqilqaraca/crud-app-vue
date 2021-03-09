//App Created By Agil

<template>
  <div id="container" class="container">
    <h3 class="text-center py-4">Contact List App</h3>
    <hr>
    <button class="btn btn-secondary show_form" @click="show_form()">{{visible ? "Show form" : "Hide form"}}</button>
    <JsonCSV
	class   = "btn btn-secondary csv"
	:data   = "userList"
	name    = "filename.csv">
  

	Download CSV

</JsonCSV>
    <div v-if="!visible" class="card">
      <div class="card-header header-form">
        Add User Form
      </div>
      <div class="card-body d-flex justify-content-between align-items-center">
        <label for=""><strong>Name :</strong></label>
        <input type="text" v-model="username"/>
        <label for=""><strong>Surname :</strong></label>
        <input type="text" v-model="usersurname"/>
        <label for=""><strong>Email :</strong></label>
        <input type="text" v-model="useremail"/>
        <button v-if="editbtn" @click="addUser()" class="btn btn-primary">Add User</button>
        <button @click="UpdateUser()" v-if="!editbtn" class="btn btn-primary">Update</button>
        <button v-if="cancel_btn" @click="cancelButton()" class="btn btn-primary">Cancel</button>
      </div>
    </div>
    <div class="card" :class="!visible ? 'mt-5' : 'mt-2'">
      <div class="card-header users">
        Users
      </div>
      <Users @EditUser = "EditUser($event)"  @DeleteUser = "DeleteUser($event)" v-for="user in userList" :key="user.id" :user="user" visible = "visible"/>
    </div>
    
  </div>
</template>

<script>

import Users from './components/Users'
import axios from 'axios'
import JsonCSV from 'vue-json-csv'

export default {
  data(){
    return{
      visible : true,
      editbtn : true,
      cancel_btn : false,
      userList : [],
      username: "",
      usersurname : "",
      useremail : "",
      userid : ""
    }
  },
  components : {
    Users,
    JsonCSV

  },
  methods:{
    show_form(){
      this.visible = !this.visible
    },
    addUser(){
      axios.post('https://contact-app-bbb43-default-rtdb.firebaseio.com/contactapp.json',{name : this.username ,surname : this.usersurname,email:this.useremail,id:this.userid})
      .then(response=>{
        this.userList.push({
          id: response.data.key,
          name : this.username,
          surname : this.usersurname,
          email : this.useremail,
          
        })
      })
      .catch(e=>{
        console.log(e)
      })
    },
    DeleteUser(value){
      axios.delete('https://contact-app-bbb43-default-rtdb.firebaseio.com/contactapp/' + value +'.json')
      .then(()=>{
        let index = this.userList.findIndex(i=>{
          return i.id  ===value
        })

        this.userList.splice(index,1)
      })
      .catch(e=>{
        console.log(e)
      })
  },
  EditUser(x){
    
    this.editbtn  = !this.editbtn,
    this.cancel_btn = !this.cancel_btn,
    this.username = x.x,
    this.usersurname = x.y,
    this.useremail = x.z,
    this.visible = false
    
  },
  cancelButton(){
    this.editbtn  = !this.editbtn,
    this.cancel_btn = false,
    this.username = "",
    this.usersurname = "",
    this.useremail = ""
  },
  },
  
  created(){
    axios.get('https://contact-app-bbb43-default-rtdb.firebaseio.com/contactapp.json')
    .then(response=>{
      for(let key in response.data){
        let user = {
          name : response.data[key].name,
          surname : response.data[key].surname,
          email : response.data[key].email,
          id : key

        }
        this.userList.push(user)
      }
    })
  }
}
</script>
    

<style>
*{
  list-style: none;
}
.header-form,.users{
  font-size: 20px;
}
.user_list p{
  margin-bottom: 0;
}
.show_form{
  margin-bottom: 15px;
}
.download{
  margin-bottom: 15px;
  margin-left: 10px;
}
.csv{
  margin-bottom: 15px;
  margin-left:10px;
}
</style>
