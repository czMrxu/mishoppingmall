<template>
  <div class="index">
    <div class="container">
      <div class="swiper-box">
        <div class="nav-menu">
          <ul class="menu-warp">
            <li class="menu-item">
              <a href="javascript:;">手机 电话卡</a>
              <div class="children">
                <ul v-for="(item, i) in menuList" :key="i">
                  <li v-for="(sub, s) in item" :key="s">
                    <a :href="sub ? '/#/product/'+sub.id : '/#/product/30'">
                      <img :src="sub ? sub.img : require('../../public/imgs/item-box-1.png')" alt="">
                      {{sub ? sub.name : '小米9'}}
                    </a>
                  </li>
                </ul>
              </div>
            </li>
            <li class="menu-item">
              <a href="javascript:;">电视盒子 电话卡</a>
            </li>
            <li class="menu-item">
              <a href="javascript:;">笔记本 平板</a>
            </li>
            <li class="menu-item">
              <a href="javascript:;">家电 插线板</a>
            </li>
            <li class="menu-item">
              <a href="javascript:;">出行 穿戴</a>
            </li>
            <li class="menu-item">
              <a href="javascript:;">智能 路由器</a>
            </li>
            <li class="menu-item">
              <a href="javascript:;">电源 配件</a>
            </li>
            <li class="menu-item">
              <a href="javascript:;">生活 箱包</a>
            </li>
          </ul>
        </div>
        <swiper :options="swiperOption">
          <swiper-slide v-for="(item, index) in slideList" :key="index">
            <a :href="'/#/product/'+item.id">
              <img :src="item.img" alt="">
            </a>
          </swiper-slide> 
          <div class="swiper-pagination" slot="pagination"></div>
          <div class="swiper-button-prev" slot="button-prev"></div>
          <div class="swiper-button-next" slot="button-next"></div>
        </swiper>
      </div>
      <div class="ads-box">
        <a :href="'/#/product/'+item.id" v-for="(item, index) in adsList" :key="index">
          <img v-lazy="item.img" alt="">
        </a>
      </div>
      <div class="banner">
        <a href="/#/product/30">
          <img v-lazy="require('../../public/imgs/banner-1.png')" alt="">
        </a>
      </div>
    </div>
    <div class="product-box">
      <div class="container">
        <h2>手机</h2>
        <div class="wrapper">
          <div class="banner-left">
            <a href="/#/product/35">
              <img v-lazy="require('../../public/imgs/mix-alpha.jpg')" alt="">
            </a>
          </div>
          <div class="list-box">
            <div class="list" v-for="(arr, a) in phoneList" :key="a">
              <div class="item" v-for="(item, i) in arr" :key="i">
                <span :class="{'new-pro' : i%2==0}">新品</span>
                <div class="item-img">
                  <img v-lazy="item.mainImage" alt="">
                </div>
                <div class="item-info">
                  <h3>{{item.name}}</h3>
                  <p>{{item.subtitle}}</p>
                  <p class="price" @click="addCart(item.id)">{{item.price}}元</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <service-bar></service-bar>
    <modal 
      title="提示" 
      sureText="查看购物车" 
      btnType="1" 
      modalType="middle"
      :showModal="showModal"
      @submit="goToCart()"
      @cancel="showModal=false">
      <template #body>
        <p>商品添加成功！</p>
      </template>
    </modal>
  </div>
</template>

<script>
  import ServiceBar from '../components/ServiceBar.vue';
  import Modal from '../components/Modal.vue';
  import { swiper, swiperSlide } from 'vue-awesome-swiper';
  import 'swiper/dist/css/swiper.css';
  
  export default {
    name: 'index',
    components: {
      swiper,
      swiperSlide,
      ServiceBar,
      Modal
    },
    data(){
      return {
        swiperOption: {
          autoplay: true,
          loop: true,
          effect: 'cube',
          cubEffect: {
            slideShadows: true,
            shadow: true,
            shadowOffset: 100,
            shadowScale: 0.6
          },
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          },
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          }
        },
        slideList: [
          {
            id: '42',
            img: require("../../public/imgs/slider/slide-1.jpg")
          },
          {
            id: '45',
            img: require("../../public/imgs/slider/slide-2.jpg")
          },
          {
            id: '46',
            img: require("../../public/imgs/slider/slide-3.jpg")
          },
          {
            id: '',
            img: require("../../public/imgs/slider/slide-4.jpg")
          },
          {
            id: '',
            img: require("../../public/imgs/slider/slide-5.jpg")
          }
        ],
        menuList: [
          [
            {
              id: 30,
              img: require('../../public/imgs/item-box-1.png'),
              name: '小米CC9'
            },
            {
              id: 31,
              img: require('../../public/imgs/item-box-2.png'),
              name: '小米8青春版'
            },
            {
              id: 32,
              img: require('../../public/imgs/item-box-3.jpg'),
              name: 'Redmi K20 Pro'
            },
            {
              id: 33,
              img: require('../../public/imgs/item-box-4.jpg'),
              name: '移动4G专区'
            }
          ],
          [0, 0, 0, 0],
          [0, 0, 0, 0],
          [0, 0, 0, 0],
          [0, 0, 0, 0],
          [0, 0, 0, 0]
        ],
        adsList: [
          {
            id: 33,
            img: require('../../public/imgs/ads/ads-1.png')
          },
          {
            id: 48,
            img: require('../../public/imgs/ads/ads-2.jpg')
          },
          {
            id: 45,
            img: require('../../public/imgs/ads/ads-3.png')
          },
          {
            id: 47,
            img: require('../../public/imgs/ads/ads-4.jpg')
          }
        ],
        phoneList: [],
        showModal: false
      }
    },
    mounted () {
      this.init();
    },
    methods: {
      init() {
        this.axios.get('/products', {
          params: {
            categoryId: 100012,
            pageSize: 14
          }
        }).then((res)=>{
          res.list = res.list.slice(6, 14);
          this.phoneList = [res.list.slice(0, 4), res.list.slice(4, 8)];
        })
      },
      addCart(id){
        if(this.$store.state.username){
          this.axios.post('/carts', {
            productId: id,
            selected: true
          }).then((res)=>{
            this.showModal = true;
            this.$store.dispatch('saveCartCount', res.cartTotalQuantity);
          })
        }else{
          window.location.href = '/#/login';
        }
      },
      goToCart(){
        this.$router.push('/cart');
      }
    },
  }
</script>

<style lang="scss">
  @import '../assets/scss/config.scss';
  @import '../assets/scss/mixin.scss';
  .index{
    .swiper-box{
      .nav-menu{
        position: absolute;
        width: 264px;
        height: 451px;
        z-index: 9;
        padding: 26px 0;
        box-sizing: border-box;
        background-color: #55585A7a;
        .menu-warp{
          .menu-item{
            height: 50px;
            line-height: 50px;
            &:hover{
              background-color: $colorA;
              .children{
                display: block;
              }
            }
            a{
              display: block;
              font-size: $fontI;
              color: $colorG;
              padding-left: 30px;
              position: relative;
              &::after{
                content: '';
                position: absolute;
                right: 30px;
                top: 17.5px;
                @include bgImg(10px, 15px, '../../public/imgs/icon-arrow.png');
              }
            }
            .children{
              display: none;
              width: 962px;
              height: 450px;
              background-color: $colorG;
              position: absolute;
              top: 0;
              left: 264px;
              border: 1px solid $colorH;
              ul{
                display: flex;
                justify-content: space-between;
                height: 75px;
                li{
                  flex: 1;
                  height: 75px;
                  line-height: 75px;
                  padding-left: 23px;
                }
                a{
                  color: $colorB;
                  font-size: $fontJ;
                }
                img{
                  width: 42px;
                  height: 35px;
                  vertical-align: middle;
                  margin-right: 15px;
                }
              }
            }
          }
        }
      }
      .swiper-container{
        height: 451px;
        .swiper-button-prev{
          left: 274px;
        }
        img{
          width: 100%;
        }
      }
    }
    .ads-box{
      @include flex();
      margin-top: 14px;
      margin-bottom: 31px;
      a{
        width: 296px;
        height: 167px;
      }
    }
    .banner{
      margin-bottom: 50px;
    }
    .product-box{
      background-color: $colorJ;
      padding: 30px 0 50px;
      h2{
        font-size: $fontF;
        height: 21px;
        line-height: 21px;
        color: $colorB;
        margin-bottom: 20px;
      }
      .wrapper{
        display: flex;
        .banner-left{
          margin-right: 16px;
          img{
            width: 224px;
            height: 619px;
          }
        }
        .list-box{
          .list{
            @include flex();
            width: 986px;
            margin-bottom: 14px;
            &:last-child{
              margin-bottom: 0;
            }
            .item{
              width: 236px;
              height: 302px;
              background-color: $colorG;
              text-align: center;
              span{
                display: inline-block;
                width: 67px;
                height: 24px;
                font-size: $fontJ;
                line-height: 24px;
                color: $colorG;
                &.new-pro{
                  background-color: #7ecf68;
                }
                &.kill-pro{
                  background-color: #e82626;
                }
              }
              .item-img{
                img{
                  height: 195px;
                  width: 100%;
                }
              }
              .item-info{
                h3{
                  font-size: $fontJ;
                  color: $colorB;
                  line-height: $fontJ;
                }
                p{
                  color: $colorD;
                  line-height: 13px;
                  margin: 6px auto 13px;
                }
                .price{
                  color: #f20a0a;
                  font-size: $fontJ;
                  font-weight: bold;
                  cursor: pointer;
                  &::after{
                    content: '';
                    margin-left: 5px;
                    vertical-align: middle;
                    @include bgImg(22px, 22px, '../../public/imgs/icon-cart-hover.png')
                  }
                }
              }
            }
          }
        }
      }
    }
  }
</style>