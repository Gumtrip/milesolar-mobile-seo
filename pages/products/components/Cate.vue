<template>
  <div id="proCate" :class="{show:popCate}" class="popModal trans">
    <ul v-if="product_categories">
      <li @click="clickCate"><h4 class="firstCate"></h4><i class="fa fa-chevron-left left"></i></li>
      <li @click="closeAll"><router-link :to="{name:'products'}"><h4 class="firstCate">All</h4></router-link></li>
      <li v-for="(cate,key) in product_categories" :key="key">
        <h4 class="firstCate" @click="toggleSecCate(key)" v-text="cate.title"></h4>
        <div v-if="cate.children" :class="{show:key===current}" class="secCate trans" @click="clickCate">
          <h5>
            <nuxt-link :to="{name:'products',query:{category_id:cate.id}}">ALL</nuxt-link>
          </h5>
          <h5 v-for="(sec,secKey) in cate.children" :key="secKey">
            <nuxt-link :to="{name:'products',query:{category_id:sec.id}}" v-text="sec.title"></nuxt-link>
          </h5>
        </div>
        <i class="fa fa-chevron-right trans right" :class="{show:key===current}"></i>
      </li>
    </ul>
  </div>
</template>

<script>
  import {productCategoryTree} from '~/plugins/http';
  export default {
  name: 'Cate',
  components: {},
  data() {
    return {
      popCate: true, // 产品分类显示
      current: null, // 当前展开的以及分类
      current_sec: 0, // 当前展开的以及分类
      product_categories: []
    }
  },
  computed: {},
  watch: {

  },
  created() {
    this.getAllCategories()
  },
  methods: {
    getAllCategories() {
      productCategoryTree({
        depth: 2
      }).then((response) => {
        this.product_categories = response.data
      })
    },
    // 关闭分类选择
    clickCate() {
      this.$emit('closePop')
    },
    // 点击一级分类展开二级分类
    toggleSecCate(index) {
      if (this.current === index) {
        this.current = null
      } else {
        this.current = index
      }
    },
    closeAll() {
      this.clickCate()
      this.current = null
    }
  }
}
</script>

<style lang="scss" scoped>
  @import "~/assets/css/_variables";
  #proCate{height: 100vh;width: 100vw;z-index: 3;left: 0;;padding-top: 50px;box-sizing: border-box;
    ul{background: #fff;height: 100%}
    li{position: relative;      border-bottom: 1px solid #ddd;margin:0 4%;
      .firstCate{
        box-sizing: border-box;align-items: center;
        height: 60px;
        line-height: 60px;
        font-size: 16px;
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;
        text-decoration: none;

      };
    }
    i{color: #464545; height: 60px;position: absolute;top: 0;
      line-height: 60px;
      &.left{left:0}
      &.right{right: 0}
      &.show{transform: rotate(90deg);}
    }
    .secCate{display: none;
      &.show{;display: block}
      a{
        display: block;
        height: 44px;
        line-height: 44px;
        font-size: 16px;
        border-top: 1px solid #ddd;
        text-indent: 20px;
        color: #464545;
        &.is-active{color: $main_green}
      }
    }
  }

</style>
