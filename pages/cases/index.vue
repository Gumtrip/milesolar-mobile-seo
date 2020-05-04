<template>
  <div class="wrapper">
    <Bread :breads="breadList"></Bread>
    <section>
      <ul
        v-infinite-scroll="getSamples"
        infinite-scroll-disabled="loading"
        infinite-scroll-distance="10"
      >
        <li v-for="(sample,key) in samples" :key="key" class="sample">
          <nuxt-link :to="{name:'cases-id',params:{id:sample.id}}">
            <div class="img flexPic">
              <img :src="sample.sm_img" alt="">
            </div>
            <div class="info">
              <h3 v-text="sample.title"></h3>
              <p v-text="sample.intro"></p>
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
import { samples } from '~/plugins/http'

export default {
  name: 'Index',
  components: { Loading, Bread },
  data() {
    return {
      samples: [],
      loading: true,
      more_data: true,
      page: 1,
      postData: {},
      breadList: [
        { title: 'Cases', url: '/cases' }
      ]
    }
  },
  computed: {},
  created() {
    this.getSamples()
  },

  methods: {
    async getSamples() {
      if (this.more_data) {
        this.loading = true
        const res = await samples({
          filter: this.postData,
          page: this.page
        })
        this.samples = this.samples.concat(res.data.data)
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
  .sample{margin-bottom: 20px;
    padding-bottom: 20px;
    border-bottom: 1px solid #ddd;
    .title{font-size: 16px;color: #555;padding-top: 8px;margin-bottom: 10px}
    .info{ font-size: 14px;color: #888;
      line-height: 150%;
    }
    .time{font-size: 14px;color: #999;line-height: 20px}
  }
</style>
