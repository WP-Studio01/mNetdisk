<template>
    <h1 v-if="logined">{{ 'Hello, ' + usrname }}</h1>
    <div v-else>
        <button @click="onLogin">Login</button>
        <button>Register</button>
        <br />
        <form action="http://netdisk.wpgzs.rf.gd/login.php" method="post" v-show="showLoginBox">
            <label>Username:</label>
            <input type="text" name="username" />
            <br />
            <label>Password:</label>
            <input type="password" name="password" />
            <input type="hidden" name="callback" v-bind:value="callback" />
            <br />
            <button type="submit">Login</button>
        </form>
    </div>
</template>

<script>
export default {
    name: "UserInfo",
    usrname: null,
    logined: false,
    showLoginBox: false,
    methods: {
        onLogin(){
            this.showLoginBox=!this.showLoginBox;
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
            showLoginBox: this.showLoginBox
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
</style>