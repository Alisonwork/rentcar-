<template>
  <div>
    <div class="orderPay">
      <Header txt="订单信息">
        <!-- <span >dhdhddd</span> -->
       <router-link  slot="left" class="leftImg" to="/rentCar"><img src="./img/back.png" /></router-link>
        <img src slot="right" class="rightImg" alt />
      </Header>
    </div>
    <div class="content">
      <div class="car_info">
        <h4>车辆信息</h4>
        <img src="./img/car.png" alt />
        <p>丰田汉兰达</p>
        <p>
          <span>SUV</span>|
          <span>2.7自动</span>|
          <span>乘坐5人</span>
        </p>
      </div>
      <div class="rent_info">
        <ul>
          <li>
            取车城市：
            <span>{{$store.state.chosedcity}}</span>
          </li>
          <li>
            取车门店：
            <span>金水路店</span>
          </li>
          <li>
            取车时间：
            <span>{{$store.state.get_time | formatDate}}</span>
          </li>
        </ul>
        <ul>
          <li>
            还车城市：
            <span>洛阳</span>
          </li>
          <li>
            还车门店：
            <span>皇城公园店</span>
          </li>
          <li>
            租车时长：
            <span>{{$store.state.tianshu}}</span>
          </li>
        </ul>
      </div>
      <div class="price">
        <p>
          天数：
          <span>{{$store.state.tianshu }}</span>
        </p>
        <p>
          定金：
          <span>300</span>元
        </p>
      </div>
      <div class="payment">
        <h4>支付方式</h4>
        <van-radio-group v-model="radio">
          <van-cell-group>
            <van-cell clickable @click="radio = '1'">
              <img src="./img/alipy.png" alt />
              <span>支付宝</span>
              <van-radio slot="right-icon" name="1" icon-size=".3rem" />
            </van-cell>
            <van-cell clickable @click="radio = '2'" icon-size=".3rem">
              <img src="./img/weixin.png" alt />
              <span>微信支付</span>
              <van-radio slot="right-icon" name="2" icon-size=".3rem" />
            </van-cell>
          </van-cell-group>
        </van-radio-group>
        <van-dropdown-menu>
          <van-dropdown-item v-model="value1" :options="option1" style="font-size:.25rem"/>
          <!-- <span>更多支付方式<img class="down" src="./img/down.png" alt=""></span> -->
         <img class="down" src="./img/down.png" alt="">
        </van-dropdown-menu>
      </div>
      <div class="agree">
  <van-checkbox v-model="checked">选中即表示您已同意 <router-link to="/clause"><span>《平价租车协议》</span></router-link></van-checkbox>
      </div>
      <div class="submit">
        <keep-alive><router-link to="/orderPay"><span  @click="submit">提交/支付</span>  </router-link></keep-alive>  
      </div>
      <div class="pay">
        <van-action-sheet v-model="show" title="付款详情">
        <p>订单编号：<span>{{$store.state.order_num}}</span></p>
        <p>支付宝账号：<span>15729384178</span></p>
        <p>付款方式：<span><span>账户余额</span> <img src="./img/icon_2.png" alt=""></span ></p>
        <p>需付款：<span>300元</span></p>
        <p @click="pay">确认付款</p>      
        </van-action-sheet>
      </div>
      <!-- 支付弹窗 -->
      <!-- <div class="num_pass">
        <van-password-input
  :value="value"
  info="密码为 6 位数字"
   @input="onInput"
  @delete="onDelete"
  @focus="showKeyboard = true"
/> -->
      <!-- </div> -->
    </div>
  </div>
</template>

<script>
import Header from "../../components/header/header.vue";
import { Toast } from 'vant';
var padDate = function(val) {
  return val < 10 ? "0" + val : val;
};
export default {
  data() {
    return {
      // tianshu:'3天',
      //   value: '',
      // showKeyboard: false,
        show: false,
      order_num:null,
      order_time:null,
      checked:true,
      radio: "1",
      value1: 0,
      option1: [
        { text: "其他支付方式", value: 0 },
        { text: "我的钱包", value: 1 },
        { text: "添加银行卡", value: 2 }
      ]
    };
  },
  filters: {
    formatDate: function(val) {
      if (val) {
        var year = val.getFullYear();
        var month = padDate(val.getMonth() + 1);
        var day = padDate(val.getDate());
        var hour = padDate(val.getHours());
        var minutes = padDate(val.getMinutes());
        var second = padDate(val.getSeconds());
        return month + "月" + day + "日";
      }
    }
  },
  methods: {
     onInput(key) {
      this.value = (this.value + key).slice(0, 6);
    },
    onDelete() {
      this.value = this.value.slice(0, this.value.length - 1);
    },
    pay(){
      console.log(99999);
      this.show=false;
      this.$router.push('/successPay')
    },
    submit(){
       if(this.checked==false){
        Toast('您还没有同意《平价租车协议》');
        console.log('nnnnnnnn');
        this.show=false;
        console.log(this.show)
      }
   else{ console.log(this.checked)

      const now = new Date();
      let y = now.getFullYear().toString();
      let m = padDate((now.getMonth()+1).toString());
      let d = padDate(now.getDate().toString());
      let h = now.getHours().toString();
      let min = now.getMinutes().toString();
      let s = now.getSeconds().toString();
      console.log(m+'-'+d)
      console.log(d+h)
      console.log(3333)
      this.order_num=y+m+d+h+min+s;
      this.order_time=y+'-'+m+'-'+d;
      this.$store.commit("orderNum",this.order_num)
      this.$store.commit("orderTime",this.order_time)
      console.log(this.order_num)
      console.log(this.order_time)
      this.show=true;
     
     }
    }
  },
  components: {
    Header
  }
};
</script>

<style scoped lang="less">
.content {
  padding: 0 0.48rem;
  overflow: hidden;
  .car_info,
  .rent_info {
    padding: 0 0.18rem;
    border-bottom: 1px solid #cccccc;
    font-size: 0.25rem;
  }
  .car_info {
    padding-top: 0.36rem;
    padding-bottom: 0.18rem;
    h4 {
      font-size: 0.3rem;
    }
    img {
      width: 1.15rem;
      height: 0.78rem;
      margin-left: 0.9rem;
      float: left;
      margin-right: 0.9rem;
    }
    p:nth-child(4) {
      margin-top: 0.1rem;
      span {
        font-size: 0.22rem;
        margin-right: 0.1rem;
        color: #000000;
      }
    }
  }
  .rent_info {
    padding-top: 0.22rem;
    padding-bottom: 0.22rem;
    font-size: 0.25rem;
    display: flex;
    flex-direction: row;
    ul {
      flex: 1;
      li:last-child {
        margin-bottom: 0;
      }
      li {
        margin-bottom: 0.12rem;
      }
    }
  }
  .price {
    font-size: 0.25rem;
    padding: 0.2rem 0.2rem 0.28rem 0.2rem;
    p:nth-child(1) {
      margin-bottom: 0.12rem;
    }
  }
  .payment {
    border-bottom: none;
    //  box-sizing: border-box;
    padding-top: 0.23rem;
    font-size: 0.25rem;
    h4 {
      font-size: 0.3rem;
      padding-left: 0.15rem;
      margin-bottom: 0.27rem;
    }
    .van-cell {
      font-size: 0.25rem;
      padding: 0.25rem 0.05rem;
      border-bottom: none;
      img {
        width: .44rem;
        height: .44rem;
        margin-right: 0.3rem;
        float: left;
        margin-top: .15rem;
      }
      span {
        display: block;
        margin-top: .1rem;
        
      }
    }
    /deep/.van-dropdown-menu{
        margin-top: .4rem;
    }
    /deep/.van-dropdown-menu__item{
    height: .5rem;
  border: none;
    }
      /deep/.van-dropdown-menu__title {
    font-size: 0.26rem;
    text-align: center;
    color: #999999
    }
    .down{
     width: .44rem;
     height: .44rem;
      margin-right: 1rem;
      float: left;
      position: absolute;
      right: .9rem;
      // top:.3rem;
      // bottom: .3rem;
      // margin-top: .1rem;
      // margin-bottom: 3rem; 
      vertical-align: middle;
     
    }
    /deep/.van-dropdown-menu__title::after{
      border: none;
      background: url('./img/down.png')
    }
    /deep/.van-radio__icon .van-icon{
      border: 1px solid #cccccc;
    }
    .van-radio__icon van-radio__icon--round van-radio__icon--checked{
      border-color: #2b5dc4
    }
    .van-radio__icon--checked .van-icon{
       border-color: #2b5dc4
    }
    }
  }
   /deep/.van-checkbox{
   height: .5rem;
  }
  .agree{
   
    float: right;
    margin-top: .58rem;
 
 
    /deep/.van-checkbox__label{
 
font-size: .22rem;
  color: #999999;
  line-height: .5rem;
    }
    /deep/.van-checkbox__icon .van-icon{
      width: .3rem;
      height: .3rem;
      font-size: .24rem;
      margin-right: .3rem;
    }
    /deep/.van-icon-success
    {
      line-height: .3rem;
      margin-top: -.02rem;
      
    }

    span{
      color: #2b5dc4;
    }
  }
  .submit{
    margin-top: 1.6rem;
    margin-bottom: .3rem;
    width: 100%;
    height: 1rem;
    font-size: .3rem;
    
    a{
      color: #ffffff;
      span{
      width: 100%;
      height: 100%;
      display: block;
      background-color: #3aa3ff;
      border-radius: .08rem;
      text-align: center;
      line-height: 1rem;
      }
    }
  }
  /deep/.van-popup--bottom{
    padding: .1rem .2rem;
    font-size: 0;
p:nth-child(4){
  color: #000000;
  border-bottom: none;
  margin-bottom: .6rem;
}
p:nth-child(5){
  width: 100%;
  text-align: center;
  height: 1rem;
 color: #ffffff;
 border-radius: .1rem;
  background: #3aa3ff;
}
    p{
    position: relative;

      font-size: .3rem;
      border-bottom: 1px solid #ccc;
      padding: .25rem 0;
      color: #999999;
      
      span{
        float: right;
        color:#000000;
      //  display: none;
      span{
        float: left;
        margin-right: .15rem;
      }
      img{
        width: .15rem;
        height: .3rem;
        margin-top: .06rem;
        vertical-align: middle;
      }
      }
      
    }
  }
</style>
