<template>
    <div class="container">
        <h2 style="text-align:center">Login</h2>
        <form id="frmLogin">
            <div id="grpEmail" class="form-group row">
                <label for="email" class="col-3 col-form-label">Email</label>
                <div class="col-9">
                    <input
                        type="email"
                        class="form-control"
                        id="email"
                        placeholder="Email"
                        required
                        maxlength="100"
                        v-model="email"
                    />
                </div>
            </div>
            <div id="grpPassword" class="form-group row">
                <label for="password" class="col-3 col-form-label">Password</label>
                <div class="col-9">
                    <input
                        type="password"
                        class="form-control"
                        id="password"
                        placeholder="Password"
                        required
                        minlength="4"
                        maxlength="15"
                        v-model="mdp"
                    />
                </div>
            </div>
            <div class="form-group row">
                <div class="col-3"></div>
                <div class="col-9">
                    <button type="submit" class="btn btn-primary" v-on:click.prevent="connexion">
                        Login
                    </button>
                </div>
            </div>
            <div class="form-group row">
                <div class="col-3"></div>
                <div id="diverror" class="col-9">
                    <div id="msgerror" class="alert alert-danger" style="display:inline-block;margin:0 auto;" role="alert">
                        {{errorText}}
                    </div>
                </div>
            </div>
        </form>
    </div>
</template>

<script>
    let srvURL = "http://tvshowapi.sv55.cmaisonneuve.qc.ca/token";
    export default {
        name: "Login",
        data : function(){
            return {
                email : "",
                mdp : "",
                errorText : null
            }
        },
        methods :{
             connexion : function(){
                 let encodedUserName = encodeURIComponent(this.email);
                 let encodedPassword = encodeURIComponent(this.mdp);
                 let bodyContent = `grant_type=password&username=${encodedUserName}&password=${encodedPassword}`;
                 fetch(`${this.$root.$data.srvURL}/token`, {
                    method: "POST",
                    body: bodyContent,
                    headers: { 'Content-Type': 'application/x-www-form-urlencoded'} 
                }).then(function (response){
                    if(!response.ok){throw response;}
                    return response.json();
                }).then((data) => {
                    console.log(data);
                    this.$root.access_token = data.access_token;
                    this.$router.push("/");
                }).catch((error) =>{
                    console.error(errorMessage);
                    errortext = error.statustext
                });
            }
        }
    };
</script>