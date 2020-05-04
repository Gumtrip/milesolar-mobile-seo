<template>
  <div id="loadingBox">
    <div class="text_center">
      <mt-spinner v-show="loading" type="snake" class="snake" color="#26a2ff"></mt-spinner>
    </div>
  </div>
</template>

<script>
import { Toast } from 'mint-ui'

export default {
  name: 'Loading',
  components: {},
  props: {
    'loading': {
      type: Boolean,
      required: true,
      default: true
    },
    'moreData': {
      type: Boolean,
      required: true,
      default: true
    }
  },
  data() {
    return {
    }
  },
  computed: {},
  watch: {
    moreData: function(newVal, oldVal) {
      if (newVal === false) {
        // Toast({
        //   message: 'No More Data!',
        //   position: 'bottom'
        // })
      }
    }
  },
  created() {

  },
  methods: {}
}
</script>
<style lang="scss" scoped>
  #loadingBox{;padding: 1vh 1vw;
    .snake{display: inline-block}
  }
</style>
