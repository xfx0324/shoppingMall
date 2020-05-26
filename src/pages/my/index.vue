<template>
  <div>
      <div class="div1">
          <div class="div1-div">
            <img src="/static/tabs/me.png" alt="" v-show="!flag">
            <img :src="imgUrl" alt="" v-show="flag">
            <p class="div1-p">
              <a href="/pages/login/main" v-show="!flag" class="login">登录</a>
              <span v-show="flag" class="login">{{name}}</span>
              <span class="qian">签到送好礼</span>
            </p>
          </div>
          <ul>
            <li>
                <p>积分</p>
                <p>资产.充值</p>
                <p>优惠劵</p>
                <p>礼品</p>
              </li>
              <li class="div1-li">
                <p>-</p>
                <p>-</p>
                <p>-</p>
                <p>-</p>
              </li>
          </ul>
      </div>
      <div class="div2">
          <p class="div2-p">我的订单</p>
          <ul>
            <li>
              <img src="/static/tabs/cart.png" alt="" @click="exitLogin">
              <img src="/static/tabs/category.png" alt="">
              <img src="/static/tabs/me.png" alt="">
              <img src="/static/tabs/cart.png" alt="">
              <img src="/static/tabs/home.png" alt="">
            </li>
            <li>
              <p>待付款</p>
              <p>待发货</p>
              <p>待收货</p>
              <p>待评价</p>
              <p>待退货</p>
            </li>
          </ul>
      </div>
      <div class="div3">
        <p class="div3-p">充值有礼</p>
        <div>
          <p>¥2000</p>
          <p>送价值89.90元赠品</p>
        </div>
      </div>
      <div class="div4">
        <ul>
            <li>
              <img src="/static/tabs/cart.png" alt="">
              <img src="/static/tabs/category.png" alt="">
              <img src="/static/tabs/me.png" alt="">
              <img src="/static/tabs/cart.png" alt="">
              <img src="/static/tabs/home.png" alt="">
            </li>
            <li>
              <p>会员中</p>
              <p>直播间</p>
              <p>企业购</p>
              <p> 客 服</p>
              <p> 卡 卷</p>
            </li>
          </ul>
      </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      flag:false, //是否登录
      name:'',
      imgUrl:'' //缓存的头像
    }
  },
  methods: {
    getName(){
      wx.getStorage({
        key:'acc',
        success:(res)=>{
          let loginArr=res.data
          this.name=loginArr[0].name
          this.imgUrl=loginArr[0].img
          this.flag=true
        }
      })
    },
    exitLogin(){
      let arr=[]
      wx.setStorage({
        key:'acc',
       data: arr
      })
      this.flag=false
    }
  },
  onShow(){
    this.getName()
  }

 
}
</script>

<style scoped>
.div1,.div2,.div3{
  width: 90%;
  margin: auto;
  box-shadow: 2px 2px 3px rgb(150, 148, 148);
  border-radius: 10px;
  font-size: 12px;
  padding:30px 10px;
  margin-bottom: 10px;
}
.div1{
  background-color:rgba(255, 255, 255, 0.938);
}
.div1-div{
  display: flex;
}
.div1-div img{
  width: 64px;
  height: 64px;
  border-radius:32px ;
}
.div1-p{
  margin-left: 20px;
}
.login{
  font-size: 22px;
  font-weight: bold;
}
.qian{
  background-color: orange;
  color: white;
  padding:3px 8px;
  border-radius: 20px;
  margin-top: 4px;
  display: block;
}
.div1 ul{
  margin-top:40px;
}
.div1 ul li,.div2 ul li,.div4 ul li{
  display: flex;
  justify-content: space-around;
}
.div1-li{
  font-weight: bolder;
}
.div2 ul{
  margin-top:20px;
}
.div2 ul li,.div4 ul li{
  margin-top:8px;
}
.div2-p,.div3-p{
  font-size: 14px;
}
.div2 ul li img,.div4 ul li img{
  width: 26px;
  height: 26px;
}
.div3 div{
  margin-top:12px;
  background-color: rgb(235, 231, 231);
  border-radius: 20px;
  width: 60%;
  padding:20px 15px;
}
.div3 div p:nth-child(1){
  font-size: 22px;
  font-weight: bold;
  margin-bottom: 8px;
}
.div3 div p:nth-child(2){
  color: orange;
}
.div4{
  width: 90%;
  margin: auto;
  font-size: 12px;
  padding:30px 10px;
  margin-bottom: 10px;
}
</style>
