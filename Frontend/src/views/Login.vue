<template>
    <div class="login">
        <div class="login-screen">
            <div class="app-title">
                <h1>Logga in</h1>
            </div>

            <div class="login-form">
                <div class="control-group">
                    <input v-model="input.name" type="text" class="login-field" value="" placeholder="användarnamn" id="login-name">
                    <label class="login-field-icon fui-user" for="login-name"></label>
                </div>

                <div class="control-group">
                    <input v-model="input.password" type="password" class="login-field" value="" placeholder="lösenord" id="login-pass" @keyup.enter="postData">
                    <label class="login-field-icon fui-lock" for="login-pass"></label>
                </div>

                <button @click="postData" class="btn btn-primary btn-large btn-block">Logga in</button>
                <br>
                <router-link to="/register">Vill du registrera dig </router-link>

            </div>
            <router-view></router-view>
        </div>

    </div>
</template>


<script>
    export default {

        data: function () {
            return {
                input:{
                password: "",
                name: "",}
            }
        },

        methods: {
            postData: async function () {
                if(this.input.name !== "" && this.input.password !== ""){
                    const requestOptions = {
                    method: "POST",
                    mode: 'cors',
                    headers: {"Content-Type": "application/json"},
                    body: JSON.stringify({user: this.input.name, pass: this.input.password})
                    };
                    const response = await fetch("http://uselessquiz.se:3000/api/users/login", requestOptions);
                    const data = await response.json();

                    if (data.length === 0) {
                        alert("Användarnamn och / eller lösenord är felaktigt!");
                    }
                    else if(data[0].user === this.input.name) {
                        this.$emit("authenticated", true);
                        this.$emit("user", data[0]);
                        await this.$router.replace({name: "mypage"});
                    } }
                else {
                    alert("Både användarnamn och lösenord måste vara ifyllda!");
                }
            },
        },

        mounted() {
            this.$parent.footerPos = 'fixed';
        }
    }
</script>

<style>
    * {
        box-sizing: border-box;
    }

    *:focus {
        outline: none;
    }

    .login {
        margin: 20px auto;
        width: 300px;
        display: flex;
        flex-direction: row;
        justify-content: center;
    }
    .login-screen {
        background-color: #FFF;
        padding: 20px;
        border-radius: 5px;
        margin-right: 10px;
        border-style: solid;
    }

    .app-title {
        text-align: center;
        color: #777;
    }

    .login-form {
        text-align: center;
    }

    .control-group {
        margin-bottom: 10px;
    }

    input {
        text-align: center;
        background-color: #ECF0F1;
        border: 2px solid transparent;
        border-radius: 3px;
        font-size: 16px;
        font-weight: 200;
        padding: 10px 0;
        width: 250px;
        transition: border .5s;
    }

    input:focus {
        border: 2px solid #3498DB;
        box-shadow: none;
    }

    .btn {
        border: 2px solid transparent;
        background: #3498DB;
        color: lightyellow;
        font-size: 16px;
        line-height: 25px;
        padding: 10px 0;
        text-decoration: none;
        text-shadow: none;
        border-radius: 3px;
        box-shadow: none;
        transition: 0.25s;
        display: block;
        width: 250px;
        margin: 0 auto;
    }

    .btn:hover {
        background-color: #2980B9;
    }
</style>