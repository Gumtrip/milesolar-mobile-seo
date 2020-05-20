<template>
  <div>
    <div id="banner">
      <swiper ref="mySwiper">
        <swiper-slide v-for="(banner,key) in banners" :key="key">
          <router-link :to="banner.url" class="bannerImg">
            <img src="../static/banner.png" alt="Power inverter, MPPT controller, off-grid solar system, MILESOLAR">
          </router-link>
        </swiper-slide>
      </swiper>
    </div>

    <section id="indexCategories">
      <ul>
        <li v-for="(category,key) in product_categories" :key="key">
          <router-link :to="{name:'products',query:{category_id:category.id}}" class="cateBox">
            <div class="cate_pic half">
              <img :src="category.mid_img">
            </div>
            <div class="cate_title half">
              <h3 class="text_center" v-text="category.title"></h3>
              <div class="link_icon">
                <i class="fa fa-arrow-circle-right"></i>
              </div>
            </div>
          </router-link>
        </li>
      </ul>
    </section>
    <section v-if="aboutsUs" id="aboutUs">
      <div v-if="aboutsUs[0]" id="imgBox">
        <div id="imgZoom" class="flexPic">
          <img :src="aboutsUs[0].sm_img" alt="">
        </div>
        <div class="top">
          <span class="left block"></span>
          <span class="right block"></span>
        </div>
        <div class="bottom">
          <span class="left block"></span>
          <span class="right block"></span>
        </div>
      </div>
      <h4 id="subTitle">A FEW WORDS ABOUT US</h4>
      <h3 id="mainTitle">A FEW WORDS ABOUT US</h3>
      <div>
        <mt-navbar v-model="selected">
          <mt-tab-item v-for="(item,index) in aboutsUs" :id="'tab-'+index" :key="index" class="a_Title">
            {{ item.title }}
          </mt-tab-item>
        </mt-navbar>
        <mt-tab-container id="detailInfo" v-model="selected">
          <mt-tab-container-item v-for="(item,key) in aboutsUs" :id="'tab-'+key" :key="key">
            <p class="txtBox" v-text="item.intro"></p>
          </mt-tab-container-item>

        </mt-tab-container>
      </div>
    </section>
    <section id="indexNews">
      <ul>
        <li v-for="(article,key) in articles" :key="key" class="list">
          <router-link :to="{name:'articles-id-slug',params:{id:article.id,slug:article.slug}}">
            <div class="txt">
              <div class="newIcon">
                <span>News</span>
              </div>
              <h3 class="newTitle" v-text="article.title"></h3>
              <div class="newDate" v-text="article.create_date"></div>
            </div>
            <div class="newPic">
              <div class="pic flexPic">
                <img :src="article.sm_img">
              </div>
            </div>
          </router-link>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
  import { productCategories, articles } from '~/plugins/http'
  import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
  import { APP_URL, TITLE } from '@/seo.config'
  import 'swiper/css/swiper.css'
  export default {
    name: 'Home',
    components: {  Swiper, SwiperSlide },
    data() {
      return {
        product_categories: [],
        articles: [],
        aboutsUs: [],
        selected: 'tab-0', // 首页about us选项卡
        banners: [
          { image: '../static/banner-1.jpg', url: '/' }
        ]
      }
    },
    computed: {},
    created() {
      this.getCategories()
      this.getNews()
      this.getAboutUs()
    },
    methods: {
      getCategories() {
        productCategories({
          depth: 1,
          sort: 'default'
        }).then((response) => {
          this.product_categories = response.data
        })
      },
      getNews() {
        articles({
          filter: { is_index: 1 },
          take: 3
        }).then((response) => {
          this.articles = response.data
        })
      },
      getAboutUs() {
        articles({
          category_id: 2,
          take: 3,
          sort: 'id'
        }).then((response) => {
          this.aboutsUs = response.data
        })
      }
    },
    head() {
      return {
        script: [{
          type: 'application/ld+json',
          json: {
            '@context': 'http://schema.org',
            '@type': 'BreadcrumbList',
            'itemListElement': [
              {
                '@type': 'ListItem',
                'position': '1',
                'name': 'Index',
                'item': APP_URL
              }
            ]
          }
        },
          {
            type: 'application/ld+json',
            json: {
              '@context': 'https://schema.org',
              '@type': 'BlogPosting',
              'headline': TITLE,
              'image': [
                APP_URL + '/logo.png'
              ]
            }
          }

        ]
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "../assets/css/variables";
  #banner{
    img{display: block;max-width: 100%;max-height: 100%}
  }
  #index_cell{display: flex;justify-content: space-between;padding: 15px}
  .cate_pic{text-align: center;
    img{max-height: 100%;max-width: 100%;}
  }
  #indexCategories{
    .cateBox{display: flex;justify-content: space-between;box-sizing: border-box;; padding: 3% 0;background-image: linear-gradient(to right, #e1e0e0 , #fffeff)}
    .half{width: 50%}
    li{position: relative;}
    .cate_title{ box-sizing: border-box;; padding:0 3%;
      h3{font-size: 24px;color: #9d9d9d;margin-bottom: 10px}
      .link_icon{}
      .link_icon i{color: $main_green;font-size: 44px}
    }
  }
  #aboutUs{padding: 65px 2% 0 2%;box-sizing: border-box;
    #imgBox{padding: 0 10px;position: relative;
      .block{position: absolute;
        width: 84px;
        height: 84px;
        background: $main_green;
        content: "";}
      .top .block{top: -10px;}
      .bottom .block{bottom: -10px;}
      .left{left: 0}
      .right{right: 0}
      #imgZoom{position: relative;z-index: 2}
    }

    #mainTitle{font-size: 32px;margin-bottom: 30px;color: #151515;}
    #subTitle{margin: 15px 0;
      font-size: 16px;
      color: #727272;
    }
    #detailInfo{margin: 25px 0 0;height: 210px;}
    .mint-navbar .mint-tab-item.is-selected{border-bottom-color: $main_green}
    .a_Title /deep/{ .mint-tab-item-label{font-size: 16px;color: #9b9b9b;}}
    .txtBox{line-height: 170%;
      font-size: 16px;
      color: #9b9b9b;}
  }
  #indexNews{width: 96%;margin: 0 auto;
    a{display: block;box-shadow: 0 0 10px #e9e9e9;}
    .list{margin-bottom: 20px;;}
    .newIcon span{background:$main_green;font-size: 14px;color: #fff;padding: 5px 10px;display: inline-block;margin-bottom: 5px;}
    .txt{box-sizing: border-box; padding: 25px 6.5% 0}
    .newTitle{max-height: 55px;font-family: "Open Sans Bold";
      font-size: 20px;
      color: #262a31;}
    .newDate{padding-top: 7px;font-size: 14px;color: #555;margin-bottom: 20px}
  }

</style>
