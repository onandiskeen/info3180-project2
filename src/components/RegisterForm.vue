<template>
<h2 class="page-header" id="pgheader">Register New User</h2>

<div class="card text-left" style="width: 48rem; " id = "card">
<div class="card-body">
<form  id = "RegisterForm" >
 
  <div class="row mb-2">
    <div class="col-auto">
      <div class="form-group">
        <label for="username">Username</label>
        <input name="username" v-model="username" placeholder="Username" class="form-control register-form">
        </div>
    </div>
    <div class="col-auto">
      <div class="form-group">
        <label for="password">Password</label>
        <input name="password" v-model="password" placeholder="Password" type="password" class="form-control register-form">
        </div>
    </div>
  </div>

  <div class="row mb-2">
    <div class="col-auto">
      <div class="form-group">
        <label for="name">Full Name</label>
        <input name="name" v-model="name" placeholder="Full Name" class="form-control register-form">
        </div>
    </div>
    <div class="col-auto">
      <div class="form-group">
        <label for="email">Email</label>
        <input name="email" v-model="email" placeholder="John.bond764@gmail.com" class="form-control register-form">
        </div>
    </div>
  </div>

    <div class="row mb-1">
        <div class="col-auto">
            <div class="form-group">
                <label for="location">Location</label>
                <input name="location" v-model="location" placeholder="" class="form-control register-form">
            </div>
        </div>
    </div>

    <div class="form-group">
        <label for="description">Biography</label>
        <textarea name="description" v-model="description"
            id="description" class="mb-2 mr-sm-2 form-control" placeholder="Add Description"></textarea>
    </div>
   
    <div class="form-group">
      <label for="photo">Upload Photo</label>
        <input class="form-control register-form" type="file" id="photo" name="photo" @change="selectImage">
    </div>
  
  <br>
  <div class="col-12">
    <button id="btn1" type="submit" name="submit" class="btn btn-primary" @click.prevent="register">Register</button>
    <button type="reset" name="reset" class="btn btn-warning">Undo all</button>
  </div>
  
</form>
</div>
</div>
</template>

<script>
export default {
    data() {
        return {
          csrf_token: '',
          description: '',
          photo: '',
          email: '',
          name: '',
          username: '',
          password: '',
          location: ''
            
        }
    },
    created() {
        this.getCsrfToken();
        
    },
  methods: {
    register(){
      
      let registerForm = document.getElementById('RegisterForm');
      let form_data = new FormData(registerForm);
      
        fetch("/api/register", {
            method: 'POST', 
            body: form_data,
            headers: {'X-CSRFToken': this.csrf_token}
          })
          .then(function (response) {
                console.log(response)
                return response.json();
          })
          .then(function (data) {
          // display a success message
            console.log("worked success")
            console.log(data);
          })
          .catch(function (error) {
              console.log(error);
          });
          this.$router.push('/login');
    },
    getCsrfToken(){
      const user = this.$store.state.auth
      console.log(user)
      let self = this;
      fetch('/api/csrf-token')
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          self.csrf_token = data.csrf_token;
        })
    },
    
  }  
}
</script>

<style>
  label {
    font-weight: bold;
    font-size: 0.8em;
  }
.register-form {
  width: 350px;
}

.page-header{
  font-size: 2em;
}

body {
  background-color: #f3f4f6;
}

</style>