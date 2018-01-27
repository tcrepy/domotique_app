<template>
  <div v-if="typeof liste.erreur === 'undefined'">
    <!--<keep-alive>-->
      <pannel v-for="(lampe, index) in listeAct" :key="index" :add="false" :nom="lampe.SW_NOM"
              :check="lampe.SW_BOOL" :id="lampe.SW_ID" :gpio="lampe.SW_GPIO_ID"
              @remove="liste.splice(index, 1)"
              @add="addElem">
      </pannel>
    <!--</keep-alive>-->
  </div>
</template>

<script>
  import Pannel from './components/pannel/pannel.vue';
  export default {
    data () {
      return {
        liste: [],
        result: [],
        etatCheck: ''
      }
    },
    mounted() {
      this.select();
    },
    computed: {
      listeAct() {
        return this.liste;
      }
    },
    methods: {
      select: function (id) {
        this.$http.post('http://149.202.41.94:8080/select', {'id': id}).then(
          response => {
            if (response.body !== this.liste){
              this.liste = response.body;
              console.log(this.liste);
            }
          },
          response => {
            this.liste = {'erreur': 'Impossible de récuperer les informations'};
          }
        )
      },
      addElem: function (value) {
        console.log(value);
        this.liste.push(value);
      }
    },
    components: {
      Pannel
    }
  }
</script>

<style>

</style>
