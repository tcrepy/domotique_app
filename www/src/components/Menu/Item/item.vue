<template>
    <li class="item" ref="item" :style="styleArr" @click="changeshowitem">
      <transition :name="currentindex === index?'item-selected':'item-not-selected'">
        <div class="item-wrapper" v-show="showitem"  @animationend="animationend">
          <button class="item-btn" :class="[icon]"></button>
        </div>
      </transition>
    </li>

</template>

<style lang="stylus" type="text/stylus">
@import "../../../common/stylus/menuConfig.styl"
.item
  opacity 1
  position absolute
  transition transform .28s cubic-bezier(.4, 0, .2, 1), box-shadow .28s cubic-bezier(.4, 0, .2, 1), opacity .28s cubic-bezier(.4, 0, .2, 1)
  width 50px
  height 50px
  border-radius 50%
  animation-duration animationDuriation
  .item-wrapper
    width 100%
    height 100%
    background-color #fafafa
    border-radius 50%

    .item-btn
      cursor pointer
      border-radius 50%
      border none
      background-color transparent
      width 100%
      height 100%
      box-shadow 0 2px 5px 0 rgba(0, 0, 0, .26)
      transition box-shadow .28s cubic-bezier(.4, 0, .2, 1)
      opacity .28s cubic-bezier(.4, 0, .2, 1)
      background-position center center
      background-repeat no-repeat
      /*opacity 0.8*/
      outline none
      &:hover
        box-shadow 0 8px 17px 0 rgba(0, 0, 0, .2)

.item-selected-leave-active
  animation-name select-item
  animation-duration animationDuriation
  animation-fill-mode forwards
.item-not-selected-leave-active
  animation-name not-select-item
  animation-duration animationDuriation
  animation-fill-mode backwards

@keyframes select-item {
  0% {
    transform scale(1)
    opacity 1
  }
  100% {
    transform scale(2)
    opacity 0
  }
}
@keyframes not-select-item {
  0% {
    transform scale(1)
    opacity 1
  }
  100% {
    transform scale(0.5)
    opacity 0
  }
}


</style>
<script type="text/ecmascript-6">
  export default {
    props: {
      radius: Number,
      anglecur: Number,
      index: Number,
      animationduration: Number,
      itemanimationdelay: Number,
      icon: String,
      showitem: Boolean,
      isopen: Boolean,
      total: Number,
      currentindex: Number
    },
    data () {
      return {
        styleArr: [],
        itemexpandanimationstyle: {
          animationName: 'expand-item-' + this.index,
          animationFillMode: 'forwards',
          animationduration: +this.animationduration + 's',
          animationDelay: this.itemanimationdelay + 's',
          animationTimingFunction: 'ease-in'
        },
        animationendCount: 0,
        itemcontractanimationstyle: {
          animationName: 'contract-item-' + this.index,
          animationFillMode: 'backwards',
          animationduration: +this.animationduration + 's',
          animationDelay: this.itemanimationdelay + 's',
          animationTimingFunction: 'ease-out'
        }
      }

    },

    computed: {
      x () {
        return this.radius * Math.cos(this.toradians(this.anglecur))
      },
      y () {
        return this.radius * Math.sin(this.toradians(this.anglecur))
      },
      x0 () {
        return 0
      },
      y0 () {
        return 0
      },
      x2 () {
        return Number((this.x).toFixed(2))
      },
      y2 () {
        return Number((this.y).toFixed(2))
      },
      x1 () {
        return this.x2 * 1.2
      },
      y1 () {
        return this.y2 * 1.2
      },
      animation () {
        if (this.isopen) {

        } else {
          return this.generateAminate()
        }
      }
    },
    watch: {
      isopen: function () {
        if (this.isopen) {
          try {
            this.styleArr.pop()
          } catch (e) {
            console.log(e)
          }
          this.styleArr.push(this.itemexpandanimationstyle)
        } else {
          this.styleArr.pop()
          this.styleArr.push(this.itemcontractanimationstyle)
        }

      }
    },

    mounted () {
      this.insertstylesheet()
    },
    methods: {
      animationend() {
        this.$emit('animationcountincrease')
      },
      changeshowitem () {
        this.$emit('showitemchange', this.index)
      },
      toradians (angle) {
        return angle * (Math.PI / 180)
      },
      generatebasekeyframe (stage) {
        let str = ''
        if (stage === 'expand-item-') {
          str = stage + this.index + '{' +
          '0% {' +
          'transform: translate(' + this.x0 + 'px,' + this.y0 + 'px)' +
          '}' +
          '70% {' +
          'transform: translate(' + this.x1 + 'px,' + this.y1 + 'px)' +
          '}' +
          '100% {' +
          'transform: translate(' + this.x2 + 'px,' + this.y2 + 'px)' +
          '}' +
          '}\n'
        } else {
          str = stage + this.index + '{' +
          '100% {' +
          'transform: translate(' + this.x0 + 'px,' + this.y0 + 'px)' +
          '}' +
          '0% {' +
          'transform: translate(' + this.x2 + 'px,' + this.y2 + 'px)' +
          '}' +
          '}\n'
        }
        return '@keyframes ' + str + '@-webkit-keyframes   ' + str

      },
      genetateanimatedetail () {

        let str = '.item-active {' +
          'animation-name: ' + 'expand-item-' + this.index + ';' +
          'animation-fill-mode: forwards;' +
          'animation-duration: 0.7s;' +
          'animation-timing-function: ease-out'
        '}\n'
        return str
      },
      insertstylesheet () {
        let cssRule = this.generatebasekeyframe('expand-item-')
        cssRule += this.generatebasekeyframe('contract-item-')
        cssRule += this.genetateanimatedetail()
        let style = document.createElement('style')
        style.type = 'text/css'
        style.innerHTML = cssRule
        document.head.appendChild(style)
      }
    }

  }
</script>
