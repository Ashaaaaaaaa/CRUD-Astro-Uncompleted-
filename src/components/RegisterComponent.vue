<template>

<div class="parent">

    <div class="wrapper">
    <div class="card-form">
        <div class="card-content">
            <h3 class="title">Register</h3>
            <form @submit.prevent="saveData">
            
            <div class="input-parent">
                <input class="input" type="text" id="username" name="name" v-model="user.name" required>
                <label class="label" id="labelusername">Name</label>
            </div>
            <div class="input-parent">
                <input class="input" type="text" id="email" name="email" v-model="user.email" required>
                <label class="label" id="labelemail">Email</label>
            </div>

            <div class="input-parent">
                <input class="input" type="password" id="password" name="password" v-model="user.password" required>
                <label class="label" id="labelpassword">Password</label>
                <i class="fa-regular fa-eye-slash" id="eye"></i>
            </div>
            <div class="input-parent">
                <input class="input" type="password" id="passwordconfirm" name="confirm_password"  v-model="user.confirm_password" required>
                <label class="label" id="labelpasswordconfirm">Confirm Password</label>
            </div>

            <div class="buttonparent">
                <div class="toregister">
                <p>
                    Already have an account? <a href="/login">Login!</a>
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
    name: 'Register',
    data () {
        return {
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
            axios.post('http://127.0.0.1:8000/api/register', this.user)
                .then(
                    ({data})=>{
                        console.log(data);
                        try {
                            alert('saved');
                        } catch (error) {
                            alert('failed');
                        }
                    }
                )
        }
    }
}
</script>
