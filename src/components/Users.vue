<template>
      <div class="card-body">
        <div class="user_list d-flex justify-content-between align-items-center">
          <p><strong>Name :</strong> {{user.name}}</p>
          <p><strong>Surname :</strong> {{user.surname}}</p>
          <p><strong>Email :</strong> {{user.email}}</p>
          <p><strong>Created time :</strong> {{new Date().toLocaleString()}}</p>
          <div class="btn_block">
          <button class="btn btn-warning" @click="$emit('EditUser',{x : user.name,y:user.surname,z:user.email,d:user.id})"  style="margin-right:14px">Edit</button>
          <button class="btn btn-danger" @click="$emit('DeleteUser',user.id)">Delete users</button>
          </div>
        </div>
      </div>
</template>

<script>
export default {
    props :["user"]
}

</script>