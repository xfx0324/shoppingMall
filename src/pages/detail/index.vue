<template>
    <div>
        <swiper autoplay="true" interval='3000' circular='true'>
       <div v-for="(item,index) in img" :key="index">
         <swiper-item>
           <image :src="'/static/'+item" class="image" mode="widthFix" />
         </swiper-item>
       </div>
     </swiper>
        <div class="div2">
            <p class="p1">{{pro.product_name}}</p>
            <p class="p2">{{pro.product_desc}}</p>
            <p class="p3">¥{{pro.price}}</p>
            <p class="p4"><span>{{detailObj.productItem.volume}}</span></p>
            <p class="p5">{{detailObj.sendTimeMsg}}</p>
            <p class="p6">
                <span>保障 48小时退换货 . 全程冷链 . 果园标准</span>
            </p>
        </div>
        <ul class="ul">
            <li v-for="(item,index) in detailObj.templateInfo.desc_imgs" :key="index">
                <image :src="'/static/'+item" alt="" class="detail" mode="widthFix" />
            </li>
        </ul>

    </div>
</template>

<script>
export default{
    data(){
        return{
            detailObj:{},
            img:'',
            pro:''
        }
    },
    methods: {
        getDetail(){
            let id1=this.$root.$mp.query.id 
            wx.request({
                url: 'http://localhost:8080/data/good_detail_'+id1+'.json',
                method: 'get',
                success: (res)=>{
                    this.detailObj = res.data
                    this.img=res.data.templatePhoto
                    this.pro=res.data.productInfo
                }
            })
        }
    },
    beforeMount(){
        this.getDetail()
    }
}
</script>

<style scoped>
    swiper{
        height: 360px;
    }
    .image{
       width: 100%;    
    }
    .div2{
        position:absolute;
        background: white;
        box-shadow: 2px 2px 3px rgb(150, 148, 148);
        width: 90%;
        margin: auto;
        left: 37.5rpx;
        top:325px;
        border-radius: 10px;
        padding-top:15px;
        padding-bottom: 15px;
    }
    .div2 p{
        text-align: center;
    }
    .p1{
        font-size: 16px;
        font-weight: bold;
    }
    .p2{
        font-size: 13px;
        color: gray;
        margin-top:5px;
    }
    .p3{
        font-size: 20px;
        font-weight: bold;
        color: orange;
        margin-top:8px;
        margin-bottom: 8px;
    }
    .p4{
        padding:8px 0;
    }
    .p4 span{
        background-color: orange;
        color: white;
        padding:8px 10px;
        border-radius: 8px;
    }
    .p5{
        color: gray;
        font-size: 10px;
        margin-top:5px;
    }
    .p6{
        font-size: 12px;
        font-weight: bold;
        margin-top: 14px;
    }
    .ul,.ul li{
        margin: 0;
        padding:0;
    }
    .ul{
        margin-top:180px;
    }
    .detail{
        width: 100%;
        vertical-align:bottom;
    }
</style>