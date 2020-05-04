<template>
  <div class="wrapper">
    <Bread :breads="breadList"></Bread>
    <section>
      <ul
        v-infinite-scroll="getArticles"
        infinite-scroll-disabled="loading"
        infinite-scroll-distance="10"
      >
        <li v-for="(article,key) in articles" :key="key" class="article">
          <nuxt-link :to="{name:'articles-id',params:{id:article.id}}">
            <div class="img flexPic">
              <img :src="article.sm_img" alt="">
            </div>
            <div class="info">
              <h3 v-text="article.title"></h3>
              <p v-text="article.create_date"></p>
            </div>
          </nuxt-link>
        </li>
      </ul>
    </section>

    <Loading :loading="loading" :more-data="more_data"></Loading>
  </div>
</template>

<script>
import Loading from '@/components/utils/Loading'
import Bread from '@/components/utils/BreadCrumb'
import { articles } from '~/plugins/http'

export default {
  name: 'Index',
  components: { Loading, Bread },
  data() {
    return {
      articles: [],
      loading: true,
      more_data: true,
      page: 1,
      postData: {},
      breadList: [
        { title: 'News', url: '/articles' }
      ]
    }
  },
  computed: {},
  created() {
    this.getArticles()
  },

  methods: {
    async getArticles() {
      if (this.more_data) {
        this.loading = true
        const res = await articles({
          filter: this.postData,
          page: this.page
        })
        this.articles = this.articles.concat(res.data.data)
        if (res.data.meta.current_page >= res.data.meta.last_page) {
          this.more_data = false
        }
        this.page++
        this.loading = false
      }
    }
  }
}
</script>

<style scoped lang="scss">
  .article{
    border-bottom: 1px solid #e5e5e5;
    margin-bottom: 15px;
    padding-bottom: 15px;
    a{display: flex;}
    .img{flex: 0 0 22%}

    .info{flex: 0 0 78%;padding-left:15px;box-sizing: border-box}
    .title{font-size: 16px;color: #555}
    .time{font-size: 14px;color: #999;line-height: 20px}

  }
</style>
