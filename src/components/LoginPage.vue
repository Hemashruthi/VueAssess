<template>
  <h1>Login to your account</h1>
<form @submit.prevent="login">
    
      <label>Username:</label>
      <input type="text" id="username" v-model="username" />
      <label>Password:</label>
      <input type="password" id="password" v-model="password" /> 
    <button>Login</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      username: null,
      password: null
    };
  },
  methods: {
    login() {
      localStorage.setItem('username',this.username);
      this.$router.push('/book')
    
    }
  }
};
</script>


<style scoped>
 form {
    position: absolute;
      width: 350px;
      margin-top: 40px; 
      background: rgba(255, 192, 203, 0.603);
      text-align: left;
      padding: 40px;
      border-radius: 10px;
      box-shadow: 0 0 3px rgba(0, 0, 0, 0.199);
    }
    label {
      color: #252424;
      display: inline-block;
      margin: 15px 0 5px;
      font-size: 0.6em;
      text-transform: uppercase;
      letter-spacing: 1px;
      font-weight: bold;
    }
    input, select {
      display: block;
      padding: 10px 6px;
      width: 100%;
      box-sizing: border-box;
      border: none;
      border-bottom: 1px solid #ddd;
      color: #555;
    }
    input[type="radio"] {
      display: inline-block;
      width: 16px;
      margin: 0 10px 0 0;
      position: relative;
      top: 2px;
    }
    input[type="checkbox"] {
      display: inline-block;
      width: 16px;
      margin: 0 10px 0 0;
      position: relative;
      top: 2px;
    }
    button {
      background: #046b10;
      border: 0;
      padding: 10px 20px;
      margin-top: 20px;
      color: white;
      border-radius: 20px;
      cursor: pointer;
    }
    .submit {
      text-align: center;
    }
    img {
      max-width:5%;
    }
    h1 {
      font-weight: 750px;
      margin-top: 0;
    }
</style>