<template>
  <v-layout row wrap justify-space-between mt-4>
      <v-flex md6 xs12 mt-4>
        <img class="image" src="~/assets/images/undraw_teaching_f1cm.png" alt="">
      </v-flex>
      
      <v-flex md6 xs12 mt-4>
        <v-row>
              <v-col
                cols="12"
                md="12"
              >
              <v-btn
              text
              >
                <span class="signup-btn ">REGISTERATION FORM</span>
              </v-btn>
              </v-col>
            </v-row>
        <v-form v-model="valid" class="login-container">
          <v-container>
            <v-row>
              <v-col
                cols="12"
                md="12"
              >
                <v-text-field
                  v-model="firstName"
                  text-align-center
                  filled
                  label="First Name"
                  type="text"
                  append-icon="mdi-information"
                ></v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col
                cols="12"
                md="12"
              >
                <v-text-field
                  v-model="lastName"
                  text-align-center
                  filled
                  label="Surname Name"
                  type="text"
                  append-icon="mdi-information"
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col
                cols="12"
                md="12"
              >
                <v-textarea
                v-model= "description"
                solo
                name="description"
                label="A short description about yourself."
                append-icon="mdi-pencil"
                ></v-textarea>
              </v-col>
            </v-row>

            <v-row>
              <v-col
                cols="12"
                md="12"
              >
                <v-text-field
                  v-model="email"
                  text-align-center
                  filled
                  label="Email"
                  type="email"
                  append-icon="mdi-email"
                ></v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col
                cols="12"
                md="12"
              >
                <v-text-field
                  filled
                  label="Password"
                  append-icon="mdi-account-lock"
                  type="password"
                  v-model="password"
                ></v-text-field>
              </v-col>

            </v-row>

            <v-row>
              <v-col
                cols="12"
                md="12"
              >
                <v-text-field
                  v-model="mobileNumber"
                  text-align-center
                  filled
                  label="mobileNumber"
                  type="number"
                  append-icon="mdi-phone"
                ></v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col
                cols="12"
                md="12"
              >
              <span>Do you have an account? <a href="/signup">Login</a></span>
              </v-col>
            </v-row>

            <v-row>
              <v-col
                cols="12"
                md="12"
              >
              <v-btn :disabled="disAbleBtn" @click.prevent="onSubmit">
                <span class="signup-btn">Signup</span>
              </v-btn>
              </v-col>
            </v-row>
          </v-container>
          <v-snackbar
            v-model="snackbar"
            timeout="5000"
          > 
            {{error}}
          </v-snackbar>
        </v-form>
      </v-flex>
  </v-layout>

  <!-- <div>
      <label for="logo" class="logo">E-LOGO</label> -->
    <!-- Header container (Section) -->
    <!-- <section> -->
      <!-- Header Image -->
      <!-- <div class="image"></div> -->
      <!-- Header writeup -->
      <!-- <div class="sect">
          <form action="">
            <div class="input-wraper">
            <font-awesome-icon class="awesome" :icon="['fas', 'user']"/> <input type="text" placeholder="First Name" required> <br>
            </div>
             <div class="input-wraper">
            <font-awesome-icon class="awesome" :icon="['fas', 'user']"/><input type="text" placeholder="Last Name" required> <br>
             </div>
             <div class="input-wraper">
            <font-awesome-icon class="awesome" :icon="['fas', 'envelope']"/> <input type="email" placeholder="Email" required> <br>
             </div>
             <div class="input-wraper">
            <font-awesome-icon class="awesome" :icon="['fas', 'lock']"/><input type="password" placeholder="Password" required> <br>
             </div>
             <div class="input-wraper">
            <font-awesome-icon class="awesome" :icon="['fas', 'lock']"/><input type="password" placeholder="Confirm Password" required> <br>
              </div>
                <div class="input-wraper">
            <button type="submit" class="btn btn-primary">SIGN UP</button> <br>
                </div>
          </form>
      </div>
    </section>
  </div> -->
</template>

<script>
export default {
    data: () => ({
      valid: false,
      firstName: '',
      lastName: '',
      description: '',
      mobileNumber: '',
      email: '',
      password: '',
      snackbar: false,
      error: '',
      disAbleBtn: false
    }),

    methods: {
      async onSubmit(){
        const userData = {
          email: this.email,
          password: this.password,
          description: this.description,
          firstName: this.firstName,
          lastName: this.lastName,
          mobileNumber: this.mobileNumber
        }
        try {
          //disable login button
          this.disAbleBtn = true;
          const details = await this.$axios.$post(
            'http://localhost:3333/api/v1/register',
            userData
          )
          // commit to store
          this.$store.dispatch('setStoreValue', details.message);

          // store in local storage
          localStorage.setItem(
              'details',
            JSON.stringify(details.message)
          )
          //enable login button
          this.disAbleBtn = false;
          this.$router.push('/courses');
        } catch (error) {

          this.disAbleBtn = false;
          this.error = 'all fields are not field yet or email already exists'
          this.snackbar = true
          setTimeout(() => this.snackbar = false, 5000 );
          console.log(error.response)
        }
      }
    }
  }

</script>

<style scoped>

.image{
  max-width: 90%;
  max-height: 100vh;
}

.login-container{
  border: .5px solid rgb(231, 225, 225);
  border-radius: 5px;
  flex-shrink: 0;
}

.login{
  text-align: center;
  display: block;
  font-weight: 900;
  color: rgb(8, 8, 180);
}

.signup-btn{
  text-align: center;
  display: block;
  font-size: large;
  font-weight: 900;
  color: rgb(8, 8, 180);
}

/*     
label.logo {
  color: #6b62fd;
  font-size: 35px;
  font-family: montserrat;
  line-height: 80px;
  padding: 0 100px;
  font-weight: bold;
} */
/* Header Container Style */

/* section {
  width: 100%;
  display: inline-flex;
  justify-items: center;
  justify-content: center;

} */

/* Header writeup style */
/* .sect {
  width: 40%;
  height: calc(100vh - 120px);
  font-size: 18px;
  padding-left: 50px;
  padding-top: 50px;
  font-weight: lighter;
  line-height: 1.4;
  margin-top: -5% ;
}

a {
  color: white;
} */
/* Header image */
/* .image {
  background: url("../assets/images/undraw_youtube_tutorial_2gn3.png") no-repeat;
  background-size: cover;
  height: calc(100vh - 200px);
  width: 40%;
  margin-right: 20px;
} */
/* form {
    position: relative;
    width: 35vw;
    height: 70vh;
    padding-top: 3em;
    border: 1px solid #C4C4C4;
    box-sizing: border-box;
    border-radius: 8px;
}
input {
    position: relative;
    width: 267px;
    height: 7vh;
    border: 1px solid rgba(107, 98, 253, 0.5);
    box-sizing: border-box;
    border-radius: 8px;
    margin-bottom: 1em;
    left: 6em;
}
button {
    position: relative;
    width: 267px;
    height: 7vh;
    left: 6.8em;
    border: none;
    background: #6B62FD;
    box-shadow: 2px 2px 10px #6B62FD;
    border-radius: 8px;
    font-weight: 600;
}
::placeholder {
    font-size: 16px;
    position: relative;
    left: 3em;
}
.awesome {
    position: absolute;
    z-index: 1;
    color: gray;
    margin-left: 7em;
    margin-top: 0.8em;
    
}

@media (max-width: 858px) {
  section {
    width: 100%;
    display: flexbox;
    flex-wrap: wrap-reverse;
    justify-items: center;
    justify-content: center;
  }
  

  .image {
   display: none;
  }
  
form {
    position: relative;
    width: 70%;
    height: 70vh;
    left:15%;
}
.sect {
  width: 100%;
  font-size: 18px;
  padding-left: 0px;
  padding-top: 0px;
 }
 
input, button {
    position: relative;
    width: 100%;
    height: 100%;
    left: 0%;
}

.input-wraper {
    position: relative;
    width: 80%;
    height: 7vh;
    margin-bottom: 1em;
    left: 10%;
}

::placeholder {
    font-size: 16px;
    position: relative;
    left: 15%;
}
.awesome {
    position: absolute;
    z-index: 1;
    color: gray;
    margin-left: 5%;
    margin-top: 0.8em;
    
}
    
label.logo {
  font-size: 25px;
  font-family: montserrat;
  padding: 0 10px;
  display: flex;
  text-align: center;
  align-items: center;
  justify-content: center;
}
} */
</style>
