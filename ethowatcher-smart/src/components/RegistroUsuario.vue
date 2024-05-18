<template>
    <div id="RegistroUsuario">
        <h2>Step 2: Register a New User</h2>

<p>To start using the program, you'll first need to create a user account. Follow these steps to complete the registration process:</p>

        <ol>
        <li>
            <strong>Locate the Registration Form:</strong> Below, you'll find the user registration form. Make sure you've completed all required fields to ensure a successful registration.
        </li>
        <li>
            <strong>Click "Register New User":</strong> Once you've filled in all the necessary fields, click the "Register New User" button to create your account. 
        </li>
        <li>
            <strong>Log In to the Program:</strong> After successfully registering, use your newly created username and password to log in. You'll find the login form at the top left corner of the page. If you encounter any issues during login, ensure that you're using the correct credentials and that your account has been successfully registered.
        </li>
        <li>
            <strong>Navigate to the Dashboard:</strong> Once logged in, head to the dashboard, where you can start organizing your data. The dashboard is your main hub for interacting with the program, and from here, you can access various features and tools to manage your data efficiently.
        </li>
        <li>
            <strong>Explore and Organize Your Data:</strong> Only you finish you can generate small subsets and use to explore your data in <a href=" http://localhost:8888/"> EthoWatcher experimentation module</a>.  
        </li>
        </ol>
                

        <div id="FormUser">
            <div>
                <input v-model="user_data['nome']" placeholder="Name">
            </div>
            <div>
                <input v-model="user_data['lab']" placeholder="Laboratory">
            </div>
            <div>
                <input v-model="user_data['login']" placeholder="Username">
            </div>
            <div>
                <input v-model="user_data['senha']" placeholder="Password">
            </div>
            <div>
                <input type="button" v-on:click="register_new_user()" value="Register new user">
            </div>
            

        </div>
    </div>
    

</template>


<script>
import { get_url } from '../services/url'

export default({
    data(){
        return{
            r_registrado:false,
            user_data:{"login": "", 
                        "lab": "", 
                        "nome": "",
                        "senha": ""}
        };
    },
    setup() {
        
    },
    methods:{
        register_new_user(){
            privada(this)
            this.user_data = {"login": "", 
                        "lab": "", 
                        "nome": "",
                        "senha": ""}

            
        }
    }
})


function privada(vue) {
  async function fun_asincrona(vue) {
    // let params = {
    //   id_experimento: id_experimento,
    // };
    let params = vue.user_data
    // let data = localStorage.getItem("access_token");

    // this = vue_component
    // vue_component.r_logado = true
    const res = await fetch(get_url () + "/api/create_user", {
      method: "POST",
      // credentials: "include",
      headers: new Headers({
        "content-type": "application/json",
        // Authorization: "Bearer " + data,
      }),
      body: JSON.stringify(params),
    });
    const data2 = await res.json();
    console.log(data2)

  }

  fun_asincrona(vue);
}
</script>

<style scoped>
input{
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}
</style>