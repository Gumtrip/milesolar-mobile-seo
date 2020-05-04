<template>
  <div>
    <div id="headerBlank" class="header"></div>
    <mt-header id="header" class="header" :class="{index:isIndex}">
      <div id="headerLeft" slot="left">
        <div id="backIcon">
          <mt-button icon="back" @click="$router.back(-1)">BACK</mt-button>
        </div>
        <div id="logo" class="logo">
          <nuxt-link to="/">
            <img src="../static/logo.png" alt="">
          </nuxt-link>
        </div>
      </div>

      <div id="function" slot="right">
        <!--        <span id="search" @click="toggleSearch(true)"><i class="fa fa-search"></i></span>-->
        <span v-show="!popNav" id="list" @click="toggleNav(true)"><i class="fa fa-list"></i></span>
        <span v-show="popNav" class="closeBtn" @click="toggleNav(false)"><i class="fa fa-close"></i></span>
      </div>

    </mt-header>
    <div id="searchBox" :class="{show:popSearch}">
      <div id="closeBtn" class="closeBtn">
        <i class="fa fa-close" @click="toggleSearch(false)"></i>
      </div>
      <form id="searchForm">
        <input type="text" placeholder="Search Our Products">
        <i id="searchBtn" class="fa fa-search"></i>
      </form>
    </div>

    <div id="nav" :class="{show:popNav}" class="popModal trans">
      <div id="menu">
        <ul>
          <li v-for="(nav,key) in navList" :key="key" class="list" @click="closeAll">
            <nuxt-link :to="nav.url" v-text="nav.title"></nuxt-link>
          </li>
        </ul>
      </div>
    </div>

    <div id="mask" :class="{show:showMask}" @click="toggleNav(false)"></div>

  </div>
</template>

<script>
export default {
  name: 'Header',
  components: {},
  data() {
    return {
      popSearch: false, // 搜索显示
      popNav: false, // 导航显示
      showMask: false, // 遮罩层
      navList: [
        { title: 'Home', url: '/' },
        { title: 'Product', url: '/products' },
        { title: 'News', url: '/articles' },
        { title: 'Cases', url: '/cases' }
      ]
    }
  },
  computed: {
    isIndex: function() {
      return this.$route.path === '/'
    }
  },
  created() {
  },
  methods: {
    toggleSearch: function(value) {
      this.popSearch = value
    },
    toggleNav: function(value) {
      if (value === true) {
        this.popNav = this.showMask = true
      } else {
        this.closeAll()
      }
    },

    closeAll: function() {
      this.popNav = this.showMask = false
    }
  }
}
</script>

<style lang="scss" scoped >
  @import "../assets/css/variables";
  .trans{transition: all 0.5s}
  .header{height: 50px;background: #fff}
  #header{position: fixed;top: 0;left: 0;width: 100vw;z-index: 2;border-bottom: 1px solid #ddd;}
  #header /deep/{
    .is-left{flex:0 0 62%}
    .is-right{flex:0 0 38%}
  }
  #headerLeft{display: flex;justify-content: space-between;}
  #function{
    i{font-size: 28px;color: $main_green}
    .closeBtn{text-align: center}
    .closeBtn i{font-size: 28px;width: 28px}
  }
  #logo{
    img{max-height: 34px}
  }
  #search{margin-right: 10px}
  #searchBox{position: fixed;top: 0;right: -100%;width: 100%; background: #fff;transition:0.5s;z-index: 3;
    &.show{right: 0;}
  }
  #searchForm{
    border-top: 1px solid #ddd;
    border-bottom: 1px solid #ddd;

    width: 100%;
    height: 60px;
    line-height: 60px;
    display: flex;
    align-items: center;
    input{border: none;box-sizing: border-box;
      width: 80%;
      height: 100%;
      padding-left: 5%;
      font-size: 16px;
      color: #c8c8c8;
      outline: none;
    }
    i{width: 20%;font-size: 30px;text-align: center}
  }
  #closeBtn{height: 50px;display: flex;align-items: center;justify-content: flex-end;box-sizing: border-box;padding-right: 10px;}
  .closeBtn{
    i {font-size: 30px;color: $main_green;}
  }

  #nav{position: fixed;top:50px;right: -100%;z-index: 3;
    &.show{right: -25%}
  }
  .popModal{height: 100vh;width: 100vw;background: #fff;}
  #mask{width: 100vw;height: 100vh;display: none;
    position: fixed;
    top: 51px;
    right: 0;
    background: rgba(0,0,0,0.5);
    z-index: 2;
    &.show{display: block}
  }
  #nav_top{display: flex;justify-content: space-between;}
  #menu {
      border-top: 1px solid #ddd;
    .list{
      margin: 0 5%;
      border-bottom: 1px solid #ddd;
    }
    a{
      display: block;
      width: 100%;
      height: 60px;
      line-height: 60px;
      font-size: 16px;
      text-decoration: none;
      color: #333;
      text-overflow: ellipsis;
      white-space: nowrap;
      overflow: hidden;
    }

  }
  #backIcon /deep/{span{color: $main_green;font-weight: bold}}
  .index #backIcon{visibility: hidden}
</style>
