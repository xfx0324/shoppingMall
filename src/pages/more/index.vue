<template>
  <div>
    <ul class="ul1">
      <li
        :class="{sortUnder: sortName === item.name}"
        v-for="(item,index) in imgArr"
        :key="index"
        @click="getSort(item.name)"
      >{{item.name}}</li>
    </ul>
    <ul class="ul2">
      <li>综合</li>
      <li>销量</li>
      <li>价格</li>
    </ul>
    <ul class="ul3">
      <li v-for="(item,index) in sortArr" :key="index">
        <img :src="'/static/'+item.photo" alt class="img" />
        <div class="ul3-div">
          <dt class="ul3-dt1">
            <p>{{item.product_name}}</p>
            <p class="des">{{item.product_desc}}</p>
          </dt>
          <dt class="ul3-dt2">
            <p class="des">{{item.volume}}</p>
            <div class="last">
              <p class="last-p">
                <span class="ul3-span1">¥{{item.price}}</span>
                <span class="ul3-span2">明日达</span>
              </p>
              <img src="/static/images/cart.png" alt class="cart" @click="addCart(item)" />
            </div>
          </dt>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imgArr: [],
      sortArr: [],
      sortName: ""
      //frist: ""
    };
  },
  methods: {
    getImg() {
      let id1 = this.$root.$mp.query.id; //this.$root.$mp.query获得的是参数的对象
      wx.request({
        url: "http://localhost:8080/data/sub_category_list_" + id1 + ".json",
        method: "get",
        success: res => {
          this.imgArr = res.data;
          let frist = this.imgArr[0].name;
          // console.log("feilei", this.frist);
          this.getSort(frist);
        }
      });
    },
    getSort(name) {
      for (let i = 0; i < this.imgArr.length; i++) {
        if (this.imgArr[i].name == name) {
          this.sortArr = this.imgArr[i].productGroup;
          this.sortName = name;
        }
      }
      console.log("name", this.sortName);
    },
    //加入购物车
    addCart(item) {
      let addPro = {
        proImg: item.photo,
        name: item.product_name,
        proPrice: item.price,
        num: 1,
        flag:false
      };
      let cartList = [];
      //判断是否登录
      wx.getStorage({
        key: "acc",
        success: res => {
          wx.getStorage({
            key: "cart",
            success: res1 => {
              cartList = res1.data;
              let index;
              let flag;
              for (let i = 0; i < cartList.length; i++) {
                if (item.product_name == cartList[i].name) {
                  flag = true;
                  index = i;
                }
              }
              if (flag) {
                cartList[index].num++;
                wx.setStorage({
                  key: "cart",
                  data: cartList
                });
              } else {
                cartList.push(addPro);
                wx.setStorage({
                  key: "cart",
                  data: cartList
                });
              }
             // console.log("index", index);
              //console.log(cartList);
            }
          });
        },
        fail: res => {
          // console.log(res)
          wx.reLaunch({ url: "/pages/login/main" });
        }
      });
      // cartList.push({"proImg":item.photo,"name":item.product_name,"proPrice":item.price})
      // console.log(cartList)
    }
  },
  beforeMount() {
    this.getImg();
  }
  //   methods: {
  //     getImg() {
  //       let id1 = this.$root.$mp.query.id; //this.$root.$mp.query获得的是参数的对象
  //       // console.log('22',this.$root)
  //       return new Promise((resolve, reject) => {
  //         wx.request({
  //           url: "http://localhost:8080/data/sub_category_list_" + id1 + ".json",
  //           method: "get",
  //           success: res => {
  //             this.imgArr = res.data;
  //             this.frist = this.imgArr[0].name;
  //             console.log("feilei", this.frist);
  //             resolve(this.frist);
  //             // this.getSort(this.frist)
  //           }
  //         });
  //       });
  //     },
  //     getSort(name) {
  //       for (let i = 0; i < this.imgArr.length; i++) {
  //         if (this.imgArr[i].name == name) {
  //           this.sortArr = this.imgArr[i].productGroup;
  //         }
  //       }
  //       console.log(this.sortArr);
  //     }
  //   },
  //   beforeMount() {
  //     // this.getImg()
  //     const that = this;
  //     async function getData() {
  //       let p = await that.getImg();
  //       p
  //         .then(d => {
  //           console.log(d, 2222);
  //         })
  //         .catch(err => {
  //           console.log(err, 3333);
  //         });
  //       console.log(that, 11111, that["frist"]);
  //       await that.getSort(that.frist);
  //       console.log(2);
  //     }
  //     getData();
  //     //console.log(111,this.frist)
  //   }
};
</script>

<style scoped>
ul {
  font-size: 12px;
  padding: 5px 9px;
  color: gray;
}
.ul1 {
  display: flex;
  justify-content: space-between;
}
.ul1 li {
  padding: 0 2px 2px 2px;
}
.sortUnder {
  border-bottom: #408000 2px solid;
  color: #408000;
}
.ul2 {
  display: flex;
  justify-content: space-between;
  padding: 5px 60px;
}
.ul3 li {
  display: flex;
  width: 750rpx;
  border-bottom: rgb(219, 216, 216) 1px solid;
  padding: 4px 0;
}
.img {
  width: 25%;
  height: 85px;
}
.ul3-div {
  width: 75%;
  margin-left: 15px;
}
.ul3-dt1 {
  margin-bottom: 14px;
}
.des {
  font-size: 10px;
  color: rgb(185, 183, 183);
}
.ul3-span1 {
  color: rgba(245, 134, 8, 0.986);
  margin-right: 10px;
}
.ul3-span2 {
  background-color: rgba(245, 134, 8, 0.986);
  padding: 2px 5px;
  color: white;
  border-radius: 5px;
}
.cart {
  width: 30px;
  height: 30px;
}
.last {
  display: flex;
}
.last-p {
  width: 82%;
}
</style>

