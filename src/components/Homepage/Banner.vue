<template>
    <div class="container">
      <div class="row">
        <div class="col-12 mx-auto">
          <div class="card card-signin flex-row">
            <div class="card-img-left d-none d-md-flex">
              <!-- Background image for card set in CSS! -->
            </div>
            <div class="card-body">
              <h5 class="card-title text-center">Become a Scholar Today!</h5>
              <form class="form-signin">
                <div class="form-label">
                  <label for="inputUserame">Username</label>
                  <input type="text" id="inputUserame" v-model="input.name" class="form-control" placeholder="Username" required autofocus>
                  <br>
                </div>
                <div class="form-label">
                  <label for="inputEmail">Email</label> 
                  <input type="email" id="inputEmail" v-model="input.email" class="form-control" placeholder="Email address" required>
                </div>
                <br>
                <div class="form-label">
                  <label for="inputPassword">Password</label>
                  <input type="password" id="inputPassword" v-model="input.password" class="form-control" placeholder="Password" required>
                </div>
                <br>
                <button class="btn btn-lg btn-primary btn-block text-uppercase" type="submit" @click.prevent="register()">Register</button>
                <router-link to="/login">Sign In</router-link>
                <hr class="my-4">
              </form>
            </div>
          </div>
        </div>
      </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      input: {
        email: "",
        password: "",
        name: ""
      }
    };
  },
  methods: {
    register() {
      let email = this.input.email;
      let password = this.input.password;
      let name = this.input.name;
      console.log(name, email, password);
      axios({
        method: "post",
        url: "http://35.194.200.110:3000/ho/signup",
        data: {
          name: name,
          email: email,
          password: password
        }
      })
        .then(response => {
          this.error_status = false;
          console.log(response.data);
          localStorage.setItem("token", response.data.token);
          this.$emit("loggedin");
          this.$router.push("questions");
        })
        .catch(err => {
          console.log(err.response);
          this.error_msg = err.response.data.message;
          this.error_status = true;
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  margin-bottom: 20px;
}

.card-signin {
  border: 0;
  border-radius: 1rem;
  box-shadow: 0 0.5rem 1rem 0 rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.card-signin .card-title {
  margin-bottom: 2rem;
  font-weight: 300;
  font-size: 1.5rem;
}

.card-signin .card-img-left {
  width: 60%;
  /* Link to your background image using in the property below! */
  background: scroll center
    url("https://www.loromedia.com/wp-content/uploads/2014/05/albert-einstein-quote.jpg");
  background-size: contain;
  background-repeat: no-repeat;
  margin-left: 10px;
}

.form-signin {
  width: 100%;
}

.form-signin .btn {
  font-size: 80%;
  border-radius: 5rem;
  letter-spacing: 0.1rem;
  font-weight: bold;
  transition: all 0.2s;
}

.form-label-group {
  position: relative;
}

.form-label-group input {
  border-radius: 2rem;
}

.btn-google {
  color: white;
  background-color: #ea4335;
}
</style>
