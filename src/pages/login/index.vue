<template>
    <div>
        <input type="text" v-model="name" placeholder="请输入登录名">
        <input type="password" v-model="password" placeholder="请输入密码">
        <button @click="login">登录</button>
        <span>{{err}}</span>
    </div>
</template>

<script>
export default{
    data(){
        return{
            name:'',
            password:'',
            err:''
        }
    },
    methods: {
        login(){
             if(this.name==="123"&&this.password==="123"){
                 let arr=[{name:'123',img:'/static/tabs/me-active.png'}]
                 let cartList=[]
                 wx.setStorage({
                key: 'acc',  //key为存储的名字  可自行定义  但是注意获取时也要用这个名字
                data: arr  //要存储的数据,是数组
        })
        wx.setStorage({
            key:'cart',
            data:cartList
        })
            // mpvue.switchTab({url:'/pages/my/main'})
            wx.reLaunch({url:'/pages/my/main'})
            // wx.reLaunch是可以跳转到tabBar页面的。它和wx.switchTab的区别就是：前者可以触发onLoad事件，而后者则不能触发。所以通过wx.reLaunch跳转到tabBar的方式完美解决了不能刷新的问题。
        }else{
            return this.err="账号或密码错误"
          }
        }
    },
    beforeMount(){

    }
}
</script>

<style scoped>
div{
    width: 80%;
    margin: auto;
    padding-top:90px;
}
/* input::-webkit-input-placeholder{
    color: rgb(177, 174, 174);
    font-size: 10px;
} */
input{
    border-bottom: 1px solid rgb(212, 211, 211);
    margin-top:50px;
    color: rgb(143, 140, 140);
    padding-left: 10px;
}
button{
    margin-top:50px;
    background-color: #408000;
    color: white;
}
span{
    color: red;
    font-size: 12px;
}
</style>