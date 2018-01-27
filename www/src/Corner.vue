<style lang="stylus" type="text/stylus">
@import "common/stylus/menuConfig.styl"
</style>
<template>
  <div>
    <bloom-menu
      @itemchange="link_to"
      :iconimgarr="iconimgarr"
      :startangle="-90"
      :endangle="0"
      :itemnum="3"
      class="menu-left-corner-wrapper"
    >
    </bloom-menu>
  </div>

</template>

<script type="text/ecmascript-6">
  import menu from './components/Menu/menu.vue'
  // 图片的icon
  let house = 'src/assets/img/icon/003-home-interface.svg';
  let add = 'src/assets/img/icon/add.svg';
  let info = 'src/assets/img/icon/002-information.svg';

  export default {
    data() {
      return {
      }
    },
    computed: {
      iconimgarr () {
        let tmp = []
        tmp.push(this.genarateIconObj(house, 'house', '50'))
        tmp.push(this.genarateIconObj(add, 'add', '50'))
        tmp.push(this.genarateIconObj(info, 'info', '50'))
        return tmp
      }
    },
    methods: {
      // 分别指定图标的url ,名称， 以及background-size属性
      genarateIconObj (url, name, size) {
        let tmp = {}
        tmp.iconUrl = url
        tmp.iconName = name
        tmp.iconSize = size
        return tmp
      },
      link_to (index) {
        switch (index) {
          case 0:
            this.$router.push('/')
            break;
          case 1:
            this.$router.push('/add')
            break;
          case 2:
            this.$router.push('/info')
            break;
        }
      }
    },
    created() {
    },
    components: {
      'bloom-menu': menu
    }
  }

</script>
