<template>
  <div class="header">
    <div class="nav-topbar">
      <div class="container">
        <div class="topbar-muenu">
          <a href="javascript:;">小米商城</a>
          <a href="javascript:;">MIUI</a>
          <a href="javascript:;">云服务</a>
          <a href="javascript:;">协议规则</a>
        </div>
        <div class="topbar-user">
          <a href="javascript:;" v-if="username">{{username}}</a>
          <a href="javascript:;" v-if="!username" @click="login">登录</a>
          <a href="/#/register" v-if="!username" @click="login">注册</a>
          <a href="javascript:;" v-if="username" @click="logout">退出</a>
          <a href="/#/order/list" v-if="username">我的订单</a>
          <a href="javascript:;" class="my-cart" @click="goToCart">
            <span class="icon-cart"></span>
            购物车({{cartCount}})
          </a>
        </div>
      </div>
    </div>
    <div class="nav-header">
      <div class="container">
        <div class="header-logo">
          <a href="/#/index"></a>
        </div>
        <div class="header-menu">
          <div class="item-menu">
            <span>小米手机</span>
            <div class="children">
              <ul>
                <li class="product" v-for="(item, index) in phoneList" :key="index">
                  <a :href="'/#/product/'+item.id" target="_blank">
                    <div class="pro-img">
                      <img v-lazy="item.mainImage" :alt="item.subtitle">
                    </div>
                    <div class="pro-name">{{item.name}}</div>
                    <div class="pro-price">{{item.price | currency}}</div>
                  </a>
                </li>
              </ul>
            </div>
          </div>
          <div class="item-menu">
            <span>RedMi红米</span>
          </div>
          <div class="item-menu">
            <span>电视</span>
            <div class="children">
              <ul>
                <li class="product">
                  <a href="" target="_blank">
                    <div class="pro-img">
                      <img v-lazy="require('../../public/imgs/nav-img/nav-3-1.jpg')" alt="">
                    </div>
                    <div class="pro-name">小米壁画电视 65英寸</div>
                    <div class="pro-price">6999元</div>
                  </a>
                </li>
                <li class="product">
                  <a href="" target="_blank">
                    <div class="pro-img">
                      <img v-lazy="require('../../public/imgs/nav-img/nav-3-2.jpg')" alt="">
                    </div>
                    <div class="pro-name">小米全面屏电视E55A</div>
                    <div class="pro-price">1999元</div>
                  </a>
                </li>
                <li class="product">
                  <a href="" target="_blank">
                    <div class="pro-img">
                      <img v-lazy="require('../../public/imgs/nav-img/nav-3-3.png')" alt="">
                    </div>
                    <div class="pro-name">小米电视4A 32英寸</div>
                    <div class="pro-price">699元</div>
                  </a>
                </li>
                <li class="product">
                  <a href="" target="_blank">
                    <div class="pro-img">
                      <img v-lazy="require('../../public/imgs/nav-img/nav-3-4.jpg')" alt="">
                    </div>
                    <div class="pro-name">小米电视4A 55英寸</div>
                    <div class="pro-price">1779元</div>
                  </a>
                </li>
                <li class="product">
                  <a href="" target="_blank">
                    <div class="pro-img">
                      <img v-lazy="require('../../public/imgs/nav-img/nav-3-5.jpg')" alt="">
                    </div>
                    <div class="pro-name">小米电视4A 65英寸</div>
                    <div class="pro-price">2699元</div>
                  </a>
                </li>
                <li class="product">
                  <a href="" target="_blank">
                    <div class="pro-img">
                      <img v-lazy="require('../../public/imgs/nav-img/nav-3-6.png')" alt="">
                    </div>
                    <div class="pro-name">查看全部</div>
                    <div class="pro-price">查看全部</div>
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <div class="header-search">
          <div class="wrapper">
            <input type="text" name="keyword">
            <a href="javascript;:"></a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapState} from 'vuex';
  export default {
    name: 'nav-header',
    data(){
      return{
        phoneList: []
      }
    },
    computed: {
      // username(){
      //   return this.$store.state.username;
      // },
      // cartCount(){
      //   return this.$store.state.cartCount;
      // }
      ...mapState(['username', 'cartCount'])
    },
    filters: {
      currency(val){
        if(!val) return '0.00';
        return val.toFixed(2) + '元起'
      }
    },
    mounted(){
      this.getProductList();
      let params = this.$route.params;
      if(params && params.from == 'login'){
        this.getCartCount();
      }
    },
    methods: {
      login(){
        this.$router.push('/login');
      },
      getProductList(){
        this.axios.get('/products', {
          params: {
            categoryId: '100012',
            pageSize: 6
          }
        }).then((res)=>{
          this.phoneList = res.list;
        })
      },
      goToCart(){
        this.$router.push('/cart');
      },
      getCartCount(){
         this.axios.get('/carts/products/sum').then((res=0)=>{
          // to-do 保存到 vuex 里面
          this.$store.dispatch('saveCartCount', res);
        })
      },
      // 退出登录
      logout(){
        this.axios.post('/user/logout').then(()=>{
          this.$message.success('退出成功');
          this.$cookie.set('userId', '', {expires: '-1'});
          this.$store.dispatch('saveUserName', '');
          this.$store.dispatch('saveCartCount', 0);
        })
      }
    }
  }
</script>

<style lang="scss">
  @import '../assets/scss/base.scss';
  @import '../assets/scss/mixin.scss';
  @import '../assets/scss/config.scss';
  .header{
    .nav-topbar{
      height: 39px;
      line-height: 39px;
      background-color: $colorB;
      color: #b0b0b0;
      .container{
        @include flex();
        a{
          display: inline-block;
          color: #b0b0b0;
          margin-right: 17px;
        }
        .my-cart{
          width: 110px;
          background-color: $colorA;
          text-align: center;
          color: $colorG;
          margin-right: 0;
          .icon-cart{
            @include bgImg(16px, 12px, '../../public/imgs/icon-cart-checked.png');
            margin-right: 4px;
          }
        }
      }
    }
    .nav-header{
      .container{
        height: 112px;
        position: relative;
        @include flex();
        .header-menu{
          display: inline-block;
          width: 643px;
          padding-left: 209px;
          .item-menu{
            display: inline-block;
            color: $colorB;
            font-weight: bold;
            font-size: $fontI;
            line-height: 112px;
            margin-right: 20px;
            span{
              cursor: pointer;
            }
            &:hover{
              color: $colorA;
              .children{
                height: 220px;
                opacity: 1;
              }
            }
            .children{
              position: absolute;
              top: 112px;
              left: 0;
              width: 1226px;
              height: 0;
              opacity: 0;
              overflow: hidden;
              border-top: 1px solid #e5e5e5;
              box-shadow: 0px 7px 6px rgba(#000, $alpha: .11);
              z-index: 10;
              transition: .5s;
              background-color: $colorG;
              .product{
                float: left;
                width: 16.6%;
                height: 220px;
                font-size: $fontK;
                line-height: 12px;
                text-align: center;
                position: relative;
                &::before{
                  content: '';
                  position: absolute;
                  top: 28px;
                  right: 0;
                  border-left: 1px solid $colorF;
                  height: 100px;
                  width: 1px;
                }
                &:last-child::before{
                  display: none;
                }
                a{
                  display: inline-block;
                }
                img{
                  width: auto;
                  height: 111px;
                  margin-top: 26px;
                }
                .pro-img{
                  height: 137px;
                }
                .pro-name{
                  font-weight: bold;
                  margin-top: 19px;
                  margin-bottom: 8px;
                  color: $colorB;
                }
                .pro-price{
                  color: $colorA;
                }
              }
            }
          }
        }
        .header-search{
          width: 319px;
          .wrapper{
            height: 50px;
            border: 1px solid #e0e0e0;
            display: flex;
            align-items: center;
            input{
              border: none;
              box-sizing: border-box;
              border-right: 1px solid #e0e0e0;
              width: 264px;
              height: 50px;
              padding-left: 14px;
            }
            a{
              @include bgImg(18px, 18px, '../../public/imgs/icon-search.png');
              margin-left: 17px;
            }
          }
        }
      }
    }
  }
</style>