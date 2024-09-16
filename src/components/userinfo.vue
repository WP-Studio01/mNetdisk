<template>
    <!-- <script src="https://challenges.cloudflare.com/turnstile/v0/api.js" async defer></script> -->
    <h1 v-if="logined">{{ 'Hello, ' + usrname }}</h1>
    <div v-else>
        <button @click="onLogin">Login</button>
        <button @click="onReg">Register</button>
        <br />
        <form action="http://wpgzs.rf.gd/login.php" method="post" v-show="showLoginBox">
            <label>Username:</label>
            <input type="text" name="username" />
            <br />
            <label>Password:</label>
            <input type="password" name="password" />
            <input type="hidden" name="callback" v-bind:value="callback" />
            <br />
            <button type="submit">Login</button>
        </form>
        <form @submit.prevent="handleSubmitReg" action="http://wpgzs.rf.gd/register.php" method="post" v-show="showRegBox">
            <label>Username:</label>
            <input type="text" name="username" />
            <br />
            <label>Password:</label>
            <input type="password" id="pas1" name="password" />
            <br />
            <label>Password agin:</label>
            <input type="password" id="pas2" />
            <br />
            <!-- <img src="http://wpgzs.rf.gd/generate_captcha.php" crossorigin="anonymous" alt="点我获取验证码" class="captcha" @click="captcha" /> -->
            <vue-turnstile site-key="0x4AAAAAAAj5HOE7l1x_WjaW" v-model="token" />
            <input type="hidden" name="token" :value="token" />
        </form>
    </div>
</template>

<script>
import VueTurnstile from 'vue-turnstile'

export default {
    name: "UserInfo",
    components: {
        VueTurnstile
    },
    usrname: null,
    logined: false,
    showLoginBox: false,
    showRegBox: false,
    token: "",
    methods: {
        onLogin(){
            this.showLoginBox=!this.showLoginBox;
            this.showRegBox=false;
        },
        onReg(){
            this.showRegBox=!this.showRegBox;
            this.showLoginBox=false;
        },
        captcha(e){
            e.target.src='http://wpgzs.rf.gd/generate_captcha.php?'+new Date().getTime();
        },
        handleSubmitReg(){
            let pas1=document.getElementById('pas1');
            let pas2=document.getElementById('pas2');
            if(pas1.value!=pas2.value){
                window.alert('password error');
                return false;
            }
            return true;
        }
    },
    onMounted(){
        let token=localStorage.getItem('token');
        // let token=1;
        if(token)
        {
            let xhr=new XMLHttpRequest();
            xhr.open('GET','http://netdisk.wpgzs.rf.gd/userinfo.php');
            xhr.setRequestHeader('Token',token);
            xhr.onreadystatechange = () => {
                if(xhr.readyState == 4)
                {
                    if(xhr.status == 200)
                    {
                        let data=xhr.responseXML;
                        this.usrname=data.getElementsByTagName('username')[0].innerHTML;
                        this.logined=true;
                    }
                }
            }
            xhr.send(null);
        }
    },
    data(){
        return {
            usrname: this.usrname,
            logined: this.logined,
            callback: window.location.href,
            showLoginBox: this.showLoginBox,
            showRegBox: this.showRegBox,
            token: this.token
        };
    }
};
</script>

<style scoped>
button {
    padding: 2px;
    margin: 5px;
    background-color: #d0d0d0;
    border-radius: 5px;
    border: none;
    cursor: pointer;
    min-width: 10%;
    height: 30px;
    font-size: large;
}
button:hover {
    background-color: #e0e0e0;
}
input {
    border: 2px solid gray;
}
input:hover {
    background-color: #f0f0f0;
}
input:focus {
    background-color: #e0e0e0;
}
.captcha {
    margin: 5px;
}
</style>