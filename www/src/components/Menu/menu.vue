<template>
  <div class="menu-container">
    <div class="menu-bar-con">
      <button class="menu-bar-btn" @click="isopenchange" v-bind:class="{'btn-active':isopen}">
        <div id="nav-icon4">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </button>
    </div>
    <ul class="menu-item-list" >
      <menu-item
        v-for="(item, itemindex) in iconimgarr"
        :radius="radius"
         :index="itemindex"
         :anglecur="startangle+anglestep*itemindex"
         :animationduration="animationduration"
         :itemanimationdelay="0 + (itemindex * itemanimationdelay)"
         :icon="'icon-'+item.iconName"
         :showitem="showitem"
         :isopen="isopen"
         :total="iconimgarr.length"
         :currentindex="currentindex"
          v-on:showitemchange="showitemchange"
          v-on:isopenchange="isopenchange"
          v-on:animationcountincrease=" (val) => {animationcountincrease(val)}"
      >
      </menu-item>

    </ul>
  </div>
</template>
<style lang="stylus" type="text/stylus">
.menu-container
  user-select none
  border-radius 50%
  transition box-shadow .28s cubic-bezier(.4, 0, .2, 1)
  box-shadow 0 2px 5px 0 rgba(0, 0, 0, .26)
  padding 0
  margin 0
  box-sizing border-box
  -webkit-tap-highlight-color rgba(0, 0, 0, 0)
  .menu-bar-con
    .menu-bar-btn
      position absolute
      border-radius 50%
      width 50px
      height 50px
      z-index 1
      cursor pointer
      transition all .28s cubic-bezier(.4, 0, .2, 1)
      border none
      background-color #74a982
      color white
      outline none
      &.btn-active
        transform rotate(45deg)
      &:hover
        box-shadow 0 8px 17px 0 rgba(0, 0, 0, .2)
      .icon
        font-size 32px
        line-height 60%
        position relative
  .menu-item-list
    list-style-type none
body .menu-container {
  left: 40px;
  bottom: 30px;
  top: auto;
  position: fixed;
  z-index: 900;
}
.menu-bar-btn {
  background: #fafafa !important;
}
.menu-bar-btn.btn-active {
  transform: rotate(0deg) !important;
}
#nav-icon4 {
  width: 26px;
  height: 45px;
  position: relative;
  margin: 13px auto;
  -webkit-transform: rotate(0deg);
  -moz-transform: rotate(0deg);
  -o-transform: rotate(0deg);
  transform: rotate(0deg);
  -webkit-transition: .5s ease-in-out;
  -moz-transition: .5s ease-in-out;
  -o-transition: .5s ease-in-out;
  transition: .5s ease-in-out;
  cursor: pointer;
}
#nav-icon4 span {
  display: block;
  position: absolute;
  height: 5px;
  width: 100%;
  background: #272f3b !important;
  border-radius: 9px;
  opacity: 1;
  left: 0;
  -webkit-transform: rotate(0deg);
  -moz-transform: rotate(0deg);
  -o-transform: rotate(0deg);
  transform: rotate(0deg);
  -webkit-transition: .25s ease-in-out;
  -moz-transition: .25s ease-in-out;
  -o-transition: .25s ease-in-out;
  transition: .25s ease-in-out;
}
#nav-icon4 span:nth-child(1) {
  top: 0;
  -webkit-transform-origin: left center;
  -moz-transform-origin: left center;
  -o-transform-origin: left center;
  transform-origin: left center;
}

#nav-icon4 span:nth-child(2) {
  top: 9px;
  -webkit-transform-origin: left center;
  -moz-transform-origin: left center;
  -o-transform-origin: left center;
  transform-origin: left center;
}

#nav-icon4 span:nth-child(3) {
  top: 18px;
  -webkit-transform-origin: left center;
  -moz-transform-origin: left center;
  -o-transform-origin: left center;
  transform-origin: left center;
}
#nav-icon4.open span:nth-child(1) {

  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  transform: rotate(45deg);
  top: 0;
  left: 3px;
}

#nav-icon4.open span:nth-child(2) {
  width: 0%;
  opacity: 0;
}

#nav-icon4.open span:nth-child(3) {
  -webkit-transform: rotate(-45deg);
  -moz-transform: rotate(-45deg);
  -o-transform: rotate(-45deg);
  transform: rotate(-45deg);
  top: 18px;
  left: 3px;
}
</style>
<script type="text/ecmascript-6">
  import Vue from 'vue'
  import item from './Item/item.vue'
  import $ from 'jquery'

  export default {
    props: {
      startangle: {
        default: 0
      },
      radius: {
        default: 115
      },
      itemanimationdelay: {
        default: 0.04
      },
      animationduration: {
        default: 0.5
      },
      endangle: {
        default: 315
      },
      itemnum: {
        default: 8
      },
      iconimgarr: Array
    },
    data () {
      return {
        showitem: true,
        isopen: false,
        total: this.iconimgarr.length,
        count: 0,
        currentindex: -1
      }
    },

    computed: {
      anglestep () {
        return (this.endangle - this.startangle) / (this.itemnum - 1)
      }
    },
    created () {
      //      把每个button的背景图片的class插入到html中,方便以后使用。
      let cssRule = ''
      this.iconimgarr.map((item) => {
        cssRule += this.gennerateIconStyle(item)
      })
      let style = document.createElement('style')
      style.type = 'text/css'
      style.innerHTML = cssRule
      document.head.appendChild(style)
      $(document).on('click', '.menu-bar-btn', function (e) {
        $('#nav-icon4').toggleClass('open');
      });
    },

    methods: {
      // 产生icon的类
      gennerateIconStyle (icon) {
        let cssRule = '.icon-' + icon.iconName + '{' +
          'background-image: url(' + icon.iconUrl + ');' +
          'background-size: ' + icon.iconSize + '%; ' +
        '}\n'
        return cssRule
      },
      animationcountincrease () {
        this.count++
        if (this.count === this.total) {
          this.isopenchange()
          this.count = 0
        }
      },
      showitemchange (index) {
        $('#nav-icon4').toggleClass('open');
        this.showitem = false
        this.currentindex = index
        this.$emit('itemchange', index)
      },
      isopenchange () {
        if (!this.isopen && !this.showitem) {
          this.showitem = true
        }
        this.isopen = !this.isopen
      },
      setAmination () {
        let angleCur = this.startangle

      }

    },
    components: {
      'menu-item': item

    }
  }
</script>
