<template>

<div class="parent">

    <div class="wrapper">
    <div class="card-form">
        <div class="card-content">
            <h3 class="title">Register</h3>
            <form @submit.prevent="loginData">
            
            <div class="input-parent">
                <input class="input" type="text" id="email" name="email" v-model="user.email" required>
                <label class="label" id="labelemail">Email</label>
            </div>

            <div class="input-parent">
                <input class="input" type="password" id="password" name="password" v-model="user.password" required>
                <label class="label" id="labelpassword">Password</label>
                <i class="fa-regular fa-eye-slash" id="eye"></i>
            </div>

            <div class="buttonparent">
                <div class="toregister">
                <p>
                  didn't have an account? please <a href="/">Register!</a>
               </p>
                </div>
             <button class="submitbtn" type="submit">Submit</button>
            </div>
            </form>
        </div>
    </div>
    </div>

</div>
</template>



<script>

import axios from 'axios';

export default {
    name: 'Login',
    data () {
        return {
            result: {},
            user:{
                email: '',
                password: '',
            }
        }
    },
    mounted(){

        const passwordInput = document.querySelector("#password")
        const usernameInput = document.querySelector("#username")
        const eye = document.querySelector("#eye")


        if(eye){
            eye.addEventListener("click", function(){
                this.classList.toggle("fa-eye")
                const type = passwordInput.getAttribute("type") === "password" ? "text" : "password"
                passwordInput.setAttribute("type", type)
                document.getElementById("password").focus();
            })
        }

        function showPass() {
            var pw = document.getElementById("password");
            if (pw.type === "password") {
                pw.type = "text";
            } else {
                pw.type = "password";
            }
        }

        $("#email").keyup(function () {
        if ($(this).val()) {
            $("#labelemail").hide();
        }
        else {
            $("#labelemail").fadeIn();
        }
        });

        $("#password").keyup(function () {
        if ($(this).val()) {
            $("#labelpassword").hide();
        }
        else {
            $("#labelpassword").fadeIn();
        }
        });

    },
    methods: {
        
        loginData(){
            axios.post('http://127.0.0.1:8000/api/login', this.user)
                .then(
                    ({data})=>{
                        console.log(data);
                        try {
                            if (data.status === true) {
                                alert("Logim Successfully");
                                window.location = "/mahasiswa";
                            } else {
                                alert("Login Failed")
                            }
                        } catch (error) {
                            alert('Error, please try again');
                        }
                    }
                )
        },

        logoutData(){
            axios.get('http://127.0.0.1:8000/api/logout')
                .then(
                    ({data})=>{
                        console.log(data);
                        try {
                            alert('Logged Out');
                        } catch (error) {
                            alert('Error, please try again');
                        }
                    }
                )
        }
    }
}
</script>
