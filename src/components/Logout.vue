<template>

<div class="parent">

</div>
</template>



<script>

import axios from 'axios';

export default {
    name: 'Login',
    mounted() {
        
        this.logoutData();
    },
    methods: {
        logoutData(){
            axios.get('http://127.0.0.1:8000/api/logout')
                .then(
                    ({data})=>{
                        console.log(data);
                        try {
                            alert('Logged Out');
                            window.location = "/";
                        } catch (error) {
                            alert('Error, please try again');
                        }
                    }
                )
        }
    }
}
</script>
