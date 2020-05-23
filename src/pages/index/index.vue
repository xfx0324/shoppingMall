<template>
  <div>
    <swiper autoplay="true" interval='3000' circular='true'>
       <div v-for="(item,index) in imgArr" :key="index">
         <swiper-item>
           <img :src="'/static/'+item" class="image"/>
         </swiper-item>
       </div>
     </swiper>
   <div v-for="(item,index) in fruits" :key="index" class="div2">
        <!-- <navigator :url="'/pages/more/main?id='+item.id"><img :src="'/static/'+item.image" alt="" class="img1"></navigator> -->
        <a :href="'/pages/more/main?id='+item.id"><img :src="'/static/'+item.image" alt="" class="img1"></a>
        <ul class="ul1">
          <li v-for="(item1,index1) in item.content" :key="index1">
             <a :href="'/pages/detail/main?id='+item1.product_id"><img :src="'/static/'+item1.image"></a>
             <dt :class="item1.banner_tag==''?'dt11':'dt1'">{{item1.banner_tag}}</dt>
             <dt>
               <p class="title">{{item1.title}}</p>
                <p class="price">¥{{item1.price}}/<span>{{item1.volume}}</span></p>
             </dt>
          </li>
        </ul>                             
   </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      imgArr:[],
      fruits:[],
    }
  },
  methods: {
    getImg(){
            wx.request({
                url: 'http://localhost:8080/data/fruits.json',
                method: 'get',
                success: (res)=>{
                    this.imgArr = res.data.banners
                    this.fruits = res.data.bannerTags
                    console.log(this.imgArr)
                }
            })
        }
  },
  beforeMount(){
    this.getImg()
  }

 
}
</script>

<style scoped>
.image{
  width: 100%;
  height:100%;
}
.div2{
  width: 750rpx;
}
.img1{
  width: 100%;
  height: 70px;
  margin-top:15px;
}
.ul1{
  display: flex;
  flex-wrap: wrap;
  width: 750rpx;
  justify-content: space-around;
}
 .ul1 li{
   font-size: 12px;
    width: 23%;
   display: flex;
   flex-direction: column;
}
.dt1{
  color: white;
  height: 17px;
  background-color: rgb(214, 92, 48);
  width:60%;
  margin: auto;
  text-align: center;
  border-radius: 4px;
}
.dt11{
  color: white;
  height: 17px;
  width:60%;
  margin: auto;
  text-align: center;
  border-radius: 4px;
}
 .ul1 li img{
   width: 90%;
   height: 85px;
} 
.price{
  color: rgb(214, 92, 48);
  text-align: center;
  font-size: 10px;
}
.title{
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.plus{
  color:white;
  background-color: rgb(214, 92, 48);
  border-radius: 3px;
  padding:1px 4px
}
</style>
