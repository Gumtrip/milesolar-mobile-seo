<template>
  <div class="wrapper">
    <Bread :breads="breadList"></Bread>
    <h3 id="cateTitle" v-text="cateTitle"></h3>
    <section id="filter">
      <button type="button" @click="showPopup">
        <label>Category</label>
        <i class="fa fa-chevron-down"></i>
      </button>
    </section>
    <ul
      id="contentBox"
      v-infinite-scroll="getProducts"
      infinite-scroll-disabled="loading"
      infinite-scroll-distance="10"
    >
      <li v-for="(product,key) in products" :key="key" class="item">
        <nuxt-link :to="{name:'products-id',params:{id:product.id}}">
          <div class="flexPic pic">
            <img :src="product.main_image" alt="">
          </div>
          <h5 class="title" v-text="product.title"></h5>
        </nuxt-link>
      </li>
    </ul>
    <mt-popup id="catePop" v-model="popupVisible" position="right">
      <Cate @closePop="closePopup"></Cate>
    </mt-popup>
    <Loading :loading="loading" :more-data="more_data"></Loading>
  </div>
</template>

<script>
import Loading from '@/components/utils/Loading'
import Bread from '@/components/utils/BreadCrumb'
import Cate from './components/Cate'
import { products, productCategory } from '~/plugins/http'

export default {
  name: 'List',
  components: {
    Loading, Cate, Bread
  },
  data() {
    return {
      products: [],
      cateTitle: 'ALL',
      loading: true,
      more_data: true,
      productCategory: {},
      page: 1,
      postData: {},
      popupVisible: false,
      breadList: [
        { title: 'Products', url: '/products' }
      ]
    }
  },
  computed: {},
  watch: {
    $route() {
      this.postData.category_id = this.$route.query.category_id
      if (this.$route.query.category_id) {
        this.getCategory(this.$route.query.category_id)
      }
      this.resetData()
      this.getProducts()
    }
  },
  created() {
    this.postData.category_id = this.$route.query.category_id
    this.getProducts()// 第一次需要手动触发
  },
  methods: {
    async getProducts() {
      if (this.more_data) {
        this.loading = true
        const res = await products({
          filter: this.postData,
          page: this.page
        })
        if(res.data){
          this.products = this.products.concat(res.data.data)
          if (res.data.meta.current_page >= res.data.meta.last_page) {
            this.more_data = false
          }
          this.page++
          this.loading = false
        }
      }
    },
    async getCategory(id) {
      const cat = await productCategory(id)
      this.productCategory = cat.data
    },
    showPopup() {
      this.popupVisible = true
    },
    closePopup() {
      this.popupVisible = false
    },
    resetData() {
      this.products = []
      this.more_data = true
      this.page = 1
    }
  }
}
</script>

<style lang="scss" scoped>
  @import "~/assets/css/_variables";
  #cateTitle{height: 37px;
    line-height: 37px;
    font-size: 24px;
    color: #000;
    font-family: "Open Sans Bold";
  }
  .blank{height: 120vh;width: 100vw}
  #contentBox{
    margin-top:34px ;
    box-sizing: border-box;;display: flex;flex-wrap: wrap;
    .item{flex:0 0 50%;box-sizing: border-box;margin-bottom: 10px;;overflow: hidden;
      a{display: block;border: 1px solid #eee;padding-bottom: 10px}
    }
    .item:nth-child(even){padding-left: 5px}
    .item:nth-child(odd){padding-right: 5px}
    .pic{margin-bottom: 10px;height: 168px;}
    .title{font-size: 16px;color: #000;width: 90%;margin: 0 auto;font-family: "Open Sans Bold";height: 56px;overflow: hidden}
  }
  #filter{
    button{
      background: $main_green;
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      border-radius: 4px;
      border: 0;
      box-sizing: border-box;
      display: block;
      font-size: 18px;
      height: 41px;
      outline: 0;
      overflow: hidden;
      position: relative;
      text-align: center;
      color: #fff;
    }
    label{margin-right: 10px}
  }
  #catePop{background: transparent}
</style>
