<template>

<div class="parent">

    <div class="wrapper">
    <div class="card-form">
        <div class="card-content">
            <h3 class="title">Register</h3>
            <ul class="alert alert-warning" v-if="Object.keys(this.errorList).length > 0">
              <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                  {{ error[0] }}
              </li>
            </ul>
            <form method="POST">
            
            <div class="input-parent">
                <input class="input" type="text" id="username" name="name" v-model="user.name">
                <label class="label" id="labelusername">Name</label>
            </div>
            <div class="input-parent">
                <input class="input" type="text" id="email" name="email" v-model="user.email">
                <label class="label" id="labelemail">Email</label>
            </div>

            <div class="input-parent">
                <input class="input" type="password" id="password" name="password" v-model="user.password">
                <label class="label" id="labelpassword">Password</label>
                <i class="fa-regular fa-eye-slash" id="eye"></i>
            </div>
            <div class="input-parent">
                <input class="input" type="password" id="passwordconfirm" name="confirm_password"  v-model="user.confirm_password">
                <label class="label" id="labelpasswordconfirm">Confirm Password</label>
            </div>

            <div class="buttonparent">
                <div class="toregister">
                <p>
                    Already have an account? <a href="/login">Login!</a>
                </p>
                </div>
             <button class="submitbtn" type="button" @click="saveData">Submit</button>
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
    name: 'Register',
    data () {
        return {
            errorList: '',
            result: {},
            user:{
                name: '',
                email: '',
                password: '',
                confirm_password: '',
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


        $("#username").keyup(function () {
        if ($(this).val()) {
            $("#labelusername").hide();
        }
        else {
            $("#labelusername").fadeIn();
        }
        });

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

        $("#passwordconfirm").keyup(function () {
        if ($(this).val()) {
            $("#labelpasswordconfirm").hide();
        }
        else {
            $("#labelpasswordconfirm").fadeIn();
        }
        });
    },
    methods: {
        
        saveData(){

            var $this = this;
            axios.post('http://127.0.0.1:8000/api/register', this.user)
                .then(res =>{

                        console.log(res.data);
                        alert(res.data.message);
                    })
                    .catch(function (error) {

                    if (error.response) {
                        if(error.response.status == 422) {

                            $this.errorList = error.response.data.errors;
                        }
                    } else if (error.request) {
                    console.log(error.request);
                    } else {
                    console.log('Error', error.message);
                    }
                })
                
        }
    }
}
</script>
