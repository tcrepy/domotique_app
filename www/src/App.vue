<template>
  <div>
    <div id="header"><h1>Domotique</h1></div>
    <div id="app_content">
      <router-view></router-view>
    </div>
    <menu-corner></menu-corner>
    <footer></footer>
  </div>
</template>
<style lang="stylus" type="text/stylus">
  #header{
    display: flex;
    justify-content: left;
    align-items: center;
    height: 100px;
    width: 100%;
    background: #272f3b;
    color: #fff;
  }
  #header h1 {
    margin-left: 20px;
    text-transform: uppercase;
    font-weight: bold;
    font-size: 1.2em;
  }
  footer{
    clear: both;
    margin-bottom: 100px;
  }
</style>
<script type="text/ecmascript-6">
  import MenuCorner from './Corner.vue';
  export default {
    props: {},


    data() {
      return {
        msg: 'hello vue'
      }
    },

    computed: {},
    created() {

    },

    methods: {
    },

    components: {
      'menu-corner': MenuCorner
    }
  }
</script>
