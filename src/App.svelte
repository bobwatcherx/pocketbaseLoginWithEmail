<script >
  import PocketBase from 'pocketbase';

const client = new PocketBase('http://localhost:8090');

let state ={
  email:"",
  password:""
}
let status = false
async function sendlogin(){
  const user = await client.Users.create({
    email: state.email,
    password: state.password,
    passwordConfirm: state.password,
});
  console.log(user)
  if(user){
    sendemail()
  }
}
async function sendemail(){
await client.Users.requestVerification(state.email);
}

//FOR LOGIN NOW
async function btnlogin(){
  const authData = await client.Users.authViaEmail(state.email, state.password);
  if(authData){
    console.log("success Login")
    status = true
  }
}

</script>
<div>
  <h1>Register</h1>
  email: <input type="text" bind:value={state.email} name="">
  <br/>
  password: <input type="password" bind:value={state.password} name="">
  <br/>
  <button
  on:click={sendlogin}
  >Register now</button>
  <hr/>


  <h1>Login</h1>
  email: <input type="text" bind:value={state.email} name="">
  <br/>
  password: <input type="password" bind:value={state.password} name="">
  <br/>
  <button
  on:click={btnlogin}
  >login now</button>
  
  <hr/>
  {#if status == true}
  <h1>You SUCCESS Login</h1>
  {/if}
  {#if status == false}
  <h1>You NOT LOGIN</h1>
  {/if}
  
</div>