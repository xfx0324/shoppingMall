<template>
  <div class="div">
      <ul class="ul1">
        <li :class="{sideLi1: sideLi === item.id}" v-for="(item,index) in sidebarList" :key="index" @click="getImg(item.id,item.name)">{{item.name}}</li>
      </ul>
      <div class="div1">
        <h3>{{name}}</h3>
        <ul class="ul2">
            <li v-for="(item,index) in imgArr" :key="index">
              <img :src="'/static/'+item.class_photo" class="image" />
              <p>{{item.name}}</p>
            </li>
        </ul>
        </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      sidebarList:[],
      imgArr:[],
      name:'新品尝鲜',
      sideLi:'478'
    }
  },
   methods: {
     getSidebar(){
         wx.request({
                url: 'http://localhost:8080/data/category_list.json',
                method: 'get',
                success: (res)=>{
                    this.sidebarList = res.data
                }
            })
     },
    getImg(id,name){
            wx.request({
                url: 'http://localhost:8080/data/category_list_'+id+'.json',
                method: 'get',
                success: (res)=>{
                    this.imgArr = (res.data)[0].classGroup
                    this.name=name
                    //console.log(this.imgArr)
                    this.sideLi=id
                }
            })
        }
  },
  beforeMount(){
    this.getSidebar()
    this.getImg('478')
  }

}
</script>

<style>
  .div{
    display: flex;
    width: 750rpx;
  }
  .ul1{
    padding:10px 0;
    background-color:#f1eeee;
    color:#bdb9b9;
    display: fixed;
    
    height: 96vh;
    /* z-index: -1; */
    font-size: 13px;
    width: 25%;

  }
  .ul1 li{
    padding:5px 10px;
  }
  .sideLi1{
    background-color: white;
    color: #408000;
  }
  .div1{
    padding-left: 20px;
    width: 75%;
    height: 96vh;
    /* z-index: -1; */
    overflow: scroll;
    
  }
  h3{
    text-align: center;
    color: gray;
  }
  .ul2{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    width: 100%;

  }
  .ul2 li{
    width: 30%;
  }
  .ul2 li img{
    width: 80%;
    height: 80px;
  }
  .ul2 li p{
    font-size: 13px;
    color: gray;
    text-align: center;
  }
</style>
