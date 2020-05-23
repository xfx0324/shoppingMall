<template>
  <div>
    <p class="p">商品清单</p>
    <ul class="ul">
      <li v-for="(item,index) in cartList" :key="index">
        <input
          type="checkbox"
          style="zoom:60%;"
          class="input"
          @click="checkAnt(item.name)"
          :checked="item.flag"
        />
        <img :src="'/static/'+item.proImg" alt class="img" />
        <dt>
          <p class="name">{{item.name}}</p>
          <div class="div">
            <span class="price">¥{{item.proPrice}}</span>
            <p class="add">
              <span @click="minusNum(item.name)">-</span>
              <input type="text" v-model="item.num" @change="editNum(item.name)" />
              <span @click="addNum(item.name)">+</span>
            </p>
          </div>
        </dt>
      </li>
    </ul>
    <div class="last">
      <p class="last-p1">
        <input type="checkbox" style="zoom:60%;" class="input" :checked="checkA" @click="checkAll()" />
        <span>全选</span>
      </p>
      <p class="last-p2">
        <span>{{totalPrice}}</span>
        <span @click="settle()">结算{{totalNum}}</span>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cartList: [],
      totalPrice: 0,
      totalNum: 0,
      checkA: false,
    };
  },
  methods: {
    getList() {
      wx.getStorage({
        key: "cart",
        success: res => {
          this.cartList = res.data;
        }
      });
    },
    //反选
    checkAnt(name) {
        let fla=true;
      for (let i = 0; i < this.cartList.length; i++) {
        if (name == this.cartList[i].name) {
            this.cartList[i].flag = !(this.cartList[i].flag)
            break;
        }
      }
      this.cartList.forEach(item => {
          if(!item.flag){
              this. checkA=false
              fla=false
          }
      })
      if(fla){
          this. checkA=true
      }
      console.log(this.cartList);
      this.totalMoney()
    },
    //全选
    checkAll() {
      this.checkA = !this.checkA;
      if (this.checkA) {
        for (let i = 0; i < this.cartList.length; i++) {
          this.cartList[i].flag = true;
        }
      } else {
        for (let i = 0; i < this.cartList.length; i++) {
          this.cartList[i].flag = false;
        }
      }
      this.totalMoney()
    //   this.cartList.forEach(ele => {
    //     ele.flag = this.checkAs;
    //   });
      // console.log(this.checkA)
    },
    //加
    addNum(name) {
      for (let i = 0; i < this.cartList.length; i++) {
        if (name == this.cartList[i].name) {
          this.cartList[i].num++;
          wx.setStorage({
                  key: "cart",
                  data: this.cartList
                });
          break;
        }
      }
      console.log(111, this.cartList);
      this.totalMoney()
    },
    //减
    minusNum(name) {
      for (let i = 0; i < this.cartList.length; i++) {
        if (name == this.cartList[i].name && this.cartList[i].num > 1) {
          this.cartList[i].num--;
          wx.setStorage({
                  key: "cart",
                  data: this.cartList
                });
          break;
        } else {
          // alert('产品数量不能小于1')
        }
      }
      this.totalMoney()
    },
    editNum(name) {
        wx.setStorage({
                  key: "cart",
                  data: this.cartList
                });
        this.totalMoney()
    },
    //总计
    totalMoney() {
        let total=0;
        let num1=0;
         this.cartList.forEach(item => {
             if(item.flag){
                 total+=Number(((item.num)*(item.proPrice)).toFixed(2))
                 num1+=Number(item.num)
             }
      })
      this.totalPrice=total
      this.totalNum=num1
    },
    //结算
    settle() {

    }
  },
  beforeMount() {
    this.getList();
  }
};
</script>

<style scoped>
.p {
  padding: 3px 4px;
  background-color: rgb(241, 239, 239);
  color: gray;
}
.ul li {
  display: flex;
  width: 750rpx;
  padding: 10px 8px;
  border-bottom: 1px solid rgb(228, 225, 225);
  justify-content: space-around;
}
.input {
  width: 6%;
}
.ul li dt {
  width: 69%;
}
.img {
  width: 20%;
  height: 70px;
}
.div {
  display: flex;
  margin-top: 30px;
  justify-content: space-between;
}
.div p {
  display: flex;
}
.name {
  font-weight: bold;
  font-size: 12px;
}
.price {
  color: orange;
  font-size: 14px;
  font-weight: bold;
  width: 30%;
}
.add {
  width: 24%;
  padding-right: 10px;
}
.add span {
  border: rgb(219, 218, 218) 1px solid;
  color: orange;
  padding: 1px 6px 2px 6px;
  border-radius: 22px;
  font-weight: bolder;
}
.add input {
  text-align: center;
}
.last {
  margin-top: 40px;
  display: flex;
  padding-left: 8px;
  padding: 13px;
  width: 750rpx;
  box-shadow: 2px 2px 3px rgb(150, 148, 148);
  justify-content: space-between;
}
.last-p1 span {
  margin-left: 20px;
  color: gray;
}
.last-p2 span:nth-child(1) {
  color: orange;
  font-weight: bold;
  margin-right: 8px;
}
.last-p2 span:nth-child(2) {
  background-color: orange;
  padding: 13px 20px;
  color: white;
}
</style>
