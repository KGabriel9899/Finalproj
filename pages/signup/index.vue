<template>
  <div class="w-full max-w-xs mx-auto sign-div">
    <form v-on:submit.prevent="signup_cred()" class="w-full max-w-lg BGC-main shadow-md rounded px-8 pt-6 pb-8 mb-4">
      <div class="flex flex-wrap -mx-3 mb-6">
        <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
          <label class="block uppercase tracking-wide text-black-700 text-xs font-bold mb-2" for="grid-first-name">
            First Name
          </label>
          <input required class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="firstname" type="text" placeholder="name">
        </div>
        <div class="w-full md:w-1/2 px-3">
          <label class="block uppercase tracking-wide text-black-700 text-xs font-bold mb-2" for="grid-last-name">
            Last Name
          </label>
          <input required class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="lastname" type="text" placeholder="lastname">
        </div>
      </div>
      <div class="flex flex-wrap -mx-3 mb-6">
        <div class="w-full px-3">
          <label class="block uppercase tracking-wide text-black-700 text-xs font-bold mb-2" for="grid-username">
            Username
          </label>
          <input required class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="username" type="text" placeholder="username">
          <!-- <p class="text-gray-600 text-xs italic">Make it as long and as crazy as you'd like</p> -->
        </div>
      </div>
      <div class="flex flex-wrap -mx-3 mb-6">
        <div class="w-full px-3">
          <label class="block uppercase tracking-wide text-black-700 text-xs font-bold mb-2" for="grid-email">
            Email
          </label>
          <input required class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="email" type="email" placeholder="email">
          <!-- <p class="text-gray-600 text-xs italic">Make it as long and as crazy as you'd like</p> -->
        </div>
      </div>
      <div class="flex flex-wrap -mx-3 mb-6">
        <div class="w-full px-3">
          <label class="block uppercase tracking-wide text-black-700 text-xs font-bold mb-2" for="grid-password">
            Password
          </label>
          <input required class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="password" type="password" placeholder="******************">
          <p class="text-black-600 text-xs italic">Make it as long and as crazy as you'd like</p>
        </div>
      </div>
      <div class="flex items-center justify-between">
            <button class="BGC-primary BGC-primary-hov text-black font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
                Sign Up
            </button>
            <!-- <a class="inline-block align-baseline font-bold text-sm text-blue-500 hover:text-blue-800" href="#">
                Forgot Password?
            </a> -->
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
let isLoggedIn = '';
export default {
  data() {
    return {
      username: '',
      password: '',
      email: '',
      firstname: '',
      lastname: ''
    }
  },
  props: {
    msg: String
  },
  mounted() {
    isLoggedIn = localStorage.getItem('jwt_token')
    if (isLoggedIn) {
      let route = this.$router.resolve({ path: "/" });
      window.open(route.href,"_self");
    }
  },
  methods: {
    signup_cred(event) {
      let users_data = {
        username: this.username,
        password: this.password,
        email: this.email,
        firstname: this.firstname,
        lastname: this.lastname
      };
      // let userlogcreds_data = {
      //   firstname: this.firstname,
      //   lastname: this.lastname
      // }
      console.log(users_data);
      // Request API.
      axios.post('http://localhost:1337/api/auth/local/register', users_data).then(response => {
        // Handle success.
        console.log('Well done!');
        console.log('User profile', response.data.user);
        console.log('User token', response.data.jwt);
        this.username = '';
        this.email = '';
        this.password= '';
        this.firstname= '';
        this.lastname= '';
        console.log(response.data);
      const formData = {
      image: '',
      id: response.data.user.id,
      description: ""
    };
    // Log the form data to the console
    console.log(formData);
    
    // Additional processing or form submission can be done here
    const options = {
      method: 'POST',
      url: 'http://localhost:1337/api/user-profiles',
      headers: {
        Authorization: 'Bearer ' + response.data.jwt,
        'content-type': 'application/json'
      },
      data: {data: {description: '', profilepic_url: '', users_id: "" + response.data.user.id}}
    };
    // console.log(options);
    axios.request(options).then(function (responsen) {
      console.log(responsen.data);
      localStorage.setItem('userprofile_id', formData.id);
    }).catch(function (error) {
      console.error(error);
    });


        window.open("http://localhost:3000/","_self");


        // const options = {
        //   method: 'POST',
        //   url: 'http://localhost:1337/api/user-log-creds',
        //   headers: {
        //     Authorization: 'Bearer ' + response.data.jwt,
        //     'content-type': 'application/json'
        //   },
        //   data: {data: {firstname: userlogcreds_data.firstname, lastname: userlogcreds_data.lastname}}
        // };
        // axios.request(options).then(function (responsen) {
        //   console.log(responsen.data);
        //   window.open("http://localhost:3000/login","_self");
        // }).catch(function (error) {
        //   // Handle error.
        //   console.log('An error occurred:', error);
        // });
      }).catch(error => {
        // Handle error.
        console.log('An error occurred:', error);
        alert('An error occurred! Check Fields' );
        this.password='';
      });  
    }
  }
}
</script>
<style scoped>
.BGC-main{
  background-color:#B28C49;
}
.BGC-primary{
  background-color: #FFEFDD;
}
.BGC-primary-hov:hover{
  background-color: #FFBF68;
}
.sign-div{
  padding-top: 50px;
}
</style>