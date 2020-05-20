<template>
  <div id="MsgBox" class="wrapper">
    <Bread :breads="breadList"></Bread>
    <div id="contactInfo">
      <h4 class="text_center">Contact Info</h4>
      <div v-for="(item,key) in contactList" :key="key">
        <mt-cell v-if="item.url" :value="item.value" :to="item.url">
          <span slot="title"><i class="fa" :class="item.icon"></i> <span class="label" v-text="item.label+':'"></span></span>
        </mt-cell>
        <mt-cell v-else :value="item.value">
          <span slot="title"><i class="fa" :class="item.icon"></i> <span class="label" v-text="item.label+':'"></span></span>
        </mt-cell>
      </div>
    </div>

    <section v-if="Object.keys(product).length > 0" id="productInfo">
      <h2 class="text_center">Product Info</h2>
      <div id="product">
        <div class="flexPic">
          <img :src="product.main_image" :alt="product.title">
        </div>
        <div class="desc">
          <h3 v-text="product.title"></h3>
        </div>
      </div>

    </section>

    <h2 class="text_center">Message</h2>
    <h3>If You Have Any Suggestions or Question For Us.Please Contact Us.</h3>
    <div id="contact_form">
      <mt-field v-model="form.name" class="input required" :attr="{required:true}" label="Name:" placeholder="Please Input Your Name">
      </mt-field>
      <mt-field v-model="form.phone" class="input" label="Mobile:" placeholder="Please Input Your Mobile" type="tel">
      </mt-field>
      <mt-field v-model="form.email" class="input required" :attr="{required:true}" label="Email:" placeholder="Please Input Your Email" type="email">
      </mt-field>
      <mt-field v-model="form.skype" class="input" label="Skype:" placeholder="Please Input Your Skype">
      </mt-field>
      <mt-field v-model="form.msg" class="input required" :attr="{required:true}" label="Message:" placeholder="Please Input Your Message" type="textarea">
      </mt-field>
      <mt-button class="submitBtn" size="large" type="primary" @click="submitForm">Submit</mt-button>
    </div>
  </div>
</template>

<script>
import { MessageBox } from 'mint-ui'
import Bread from '@/components/utils/BreadCrumb'
import { product, storeMsg } from '~/plugins/http'
import { APP_URL } from '~/seo.config'

export default {
  name: 'Index',
  components: { Bread },
  data() {
    return {
      contactList: [
        { label: 'Email', icon: 'fa-envelope', value: 'info@milesolar.com', url: 'mailto:info@milesolar.com' },
        { label: 'Phone', icon: 'fa-mobile', value: '008613889943867', url: 'tel:008613889943867' },
        { label: 'Skype', icon: 'fa-skype', value: '13812341234@qq.com' }
      ],
      form: {
        product_id: null,
        product_info: null,
        name: '',
        phone: '',
        email: '',
        skype: '',
        msg: ''
      },
      product: [],
      breadList: [
        { title: 'Products', url: '/products' }
      ]
    }
  },
  computed: {},
  watch: {
    $route() {
      this.$router.go(0)
    }
  },
  created() {
    const query = this.$route.query
    this.form.product_id = query.product_id

    if (this.form.product_id) {
      this.getProduct(this.form.product_id)
    }
  },
  methods: {
    async submitForm() {
      try {
        const res = await storeMsg(this.form)
        if (res.status === 201) {
          MessageBox({
            'title': 'Message',
            'message': 'Thanks For Your Message!',
            'confirmButtonText': 'OK'
          }).then(() => {
            this.form.name = ''
            this.form.phone = ''
            this.form.email = ''
            this.form.skype = ''
            this.form.msg = ''
          })
        }
      } catch (e) {
        console.log(e)
      }
    },
    getProduct(id) {
      product(id).then((response) => {
        this.product = response.data
        this.form.product_id = id
        this.form.product_info = response.data
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
              'position': 1,
              'name': 'Index',
              'item': APP_URL
            },
            {
              '@type': 'ListItem',
              'position': 2,
              'name': 'Contact Us',
              'item': APP_URL + '/contact'
            }
          ]
        }
      }
      ]
    }
  }

}
</script>

<style lang="scss" scoped>
  @import "~/assets/css/_variables";
  #MsgBox {padding-top: 20px;
    .input /deep/ {
      &.required .mint-cell-title:after{content: '*';color: $red}
      input,textarea{border: 1px solid $light_grey;border-radius: 5px;padding: 6px 10px;box-sizing: border-box}}
    h2{margin-bottom: 10px}
    h3{margin-bottom: 20px;font-size: 16px;color: #666;padding-left:  10px}
    p{padding-left: 10px;color: #555}
    .submitBtn{margin-top: 10px;}
  }
  #productInfo{margin-bottom: 10px;box-sizing: border-box;;
    #product{display: flex;
      .flexPic{border: 1px solid $light_grey;border-radius: 5px;width: 35%;box-sizing: border-box;overflow: hidden}
      .desc{width: 65%;box-sizing: border-box;padding-left: 10px}
    }
    h3{padding: 5px 0;font-size: 16px;color: $grey}
  }

  #contactInfo{margin-bottom: 20px;
    .fa{display: inline-block;width: 20px;}
    .fa-mobile{font-size: 26px}
    .fa-skype{font-size: 20px}
    h4{margin-bottom: 10px;font-size: 24px}
    color: #555;
  }
  #contact_form{margin-bottom: 20px}
</style>
