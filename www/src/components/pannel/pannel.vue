<template>
  <transition name="destroy">
    <div class='contenant' v-if="isShow === true">
      <div class='card'>
        <transition name='rotateZ' appear>
          <div class='pannel' v-if='!isVisible'>
            <div id='lamp' v-if='add === false'>
              <input type='checkbox' name='switch' :checked='this.check == 1' @click='update(id, $event)'/>
              <div class='lamp'>
                <div class='gonna-give-light'></div>
              </div>
              <div class='nom_lampe'>
                <div class='container'>
                  <h4>{{this.oldName}}</h4>
                  <button class='icon' @click='visible'><i class='fa fa-cog'></i></button>
                </div>
              </div>
            </div>
          </div>
        </transition>
        <transition name='rotateZ'>
          <div class='dialog' v-if='isVisible'>
            <div v-if="add === false">
              <div class='params'>
                <div class='inputs'>
                  <input type='text' name='newName' v-model='newName' placeholder='Nouveau nom'
                         @keydown.enter="updateName(id)">
                </div>
                <button class="button" @click='updateName(id)'>Changer le nom</button>
              </div>
              <div class='suppression'>
                <button class="button" @click='supprimer(id)'>Supprimer la lampe</button>
              </div>
              <i class='icon icon-bs fa fa-remove' @click='visible'></i>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </transition>
</template>
<script>
  export default {
    props: {
      add: {default: false},
      nom: String,
      check: {default: 0},
      id: Number,
      gpio: Number
    },
    data() {
      return {
        checkVal: this.check,
        isVisible: false,
        newName: '',
        oldName: this.nom,
        pgpio: '',
        isShow: true
      }
    },
    methods: {
      update: function (id, event) {
        this.$http.post('http://149.202.41.94:8080/update', {'id': id, 'bool': event.target.checked}).then(
          response => {
            this.result = response.body;
          },
          response => {
            this.result = {'erreur': 'Impossible de mettre à jour la bdd'};
            console.log('ERREUR');
          }
        )
      },
      visible() {
        this.isVisible = !this.isVisible;
      },
      show() {
        this.isShow = !this.isShow;
      },
      updateName(id) {
        this.$http.post('http://149.202.41.94:8080/updatename', {'id': id, 'newName': this.newName}).then(
          response => {
            this.result = response.body;
            this.oldName = this.newName;
            this.newName = '';
          },
          response => {
            this.result = {'erreur': 'Impossible de mettre à jour la bdd'};
            console.log('ERREUR');
          }
        );
        this.visible();
      },
      supprimer(id) {
        this.$http.post('http://149.202.41.94:8080/supprimer', {'id': id}).then(
          response => {
            this.result = response.body;
          },
          response => {
            this.result = {'erreur': 'Impossible de mettre à jour la bdd'};
            console.log('ERREUR');
          }
        );
        this.show();
        setTimeout(function () {
          this.$emit('remove');
        }, 800);
      },
      addLamp() {
        this.$http.post('http://149.202.41.94:8080/add', {'name': this.newName, 'pgpio': this.pgpio}).then(
          response => {
            this.result = response.body;
            this.newName = '';
            this.pgpio = '';
            this.$http.post('http://149.202.41.94:8080/select', {'id': response.body.results.insertId}).then(
              response => {
                this.visible();
                this.$parent.$data.liste.push(response.body[0]);
              },
              response => {
                this.liste = {'erreur': 'Impossible de récuperer les informations'};
              }
            );
          },
          response => {
            this.result = {'erreur': 'Impossible de mettre à jour la bdd'};
            console.log('ERREUR');
          }
        );
      }
    }
  }
</script>

<style>
  .contenant {
    display: block;
    position: relative;
    float: left;
    margin: 10px;
    z-index: 1;
    perspective: 800px;
  }

  .card{
    width: 100%;
    height: 100%;
    position: absolute;
    transform-style: preserve-3d;
    transition: transform 1s;
    transform-origin: right center;
  }

  .card > div {
    margin: 0;
    display: block;
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border-radius: 20px;
    box-shadow: 6px 6px 12px #555;
  }

  .pannel {
    display: inline-block;
    background: #272f3b;
    position: relative;
    z-index: 1;
  }

  @import url(http://weloveiconfonts.com/api/?family=entypo);
  *, *:before, *:after {
    margin:0;
    padding:0;
    -webkit-box-sizing:border-box;
    -moz-box-sizing:border-box;
    box-sizing:border-box;
  }
  #lamp {
    position:relative;
    width:100%;
    height:100%;
    overflow:hidden;
  }
  input[name="switch"], input[name="switch"] + label {
    position:absolute;
    bottom:23%;
    left:0;
    width:100%;
    height:77%;
  }
  input[name="switch"] + label {right:3rem;}
  input[name="switch"] {
    opacity:0;
    z-index:9;
    cursor:pointer;
  }
  input[name="switch"] + label {
    text-align:center;
  }
  [class*="entypo-"]:before {
    line-height:4rem;
    font-size:2.5rem;
    color:rgba(255,255,255,0.4);
    font-family:'entypo', sans-serif;
  }
  .lamp {
    position:relative;
    margin:0 auto;
    width:.7rem;
    height:4rem;
    background-image:linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)),
    linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)),
    linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7));
    background-repeat:no-repeat;
    background-size:.15rem 2rem, .4rem .8rem, .7rem 2rem;
    background-position:50% 0, .15rem 2rem, 0 2.8rem;
  }
  .lamp:before, .lamp:after {
    content:'';
    position:absolute;
  }
  .lamp:before {
    left:-1.65rem;
    bottom:-4rem;
    width:4rem;
    height:4rem;
    border-radius:50%;
    background:rgba(255,255,255,0.03);
    box-shadow:inset 2px -2px 10px rgba(255,255,255,0.07);
    transition:all 0.6s;
  }
  .gonna-give-light,
  .gonna-give-light:before{
    position:absolute;
  }
  .gonna-give-light {
    top:4.2rem;
    left:.25rem;
    width:0;
    height:1.5rem;
    border-right:.2rem solid rgba(255,255,255,0.05);
  }
  .gonna-give-light:before {
    content:'';
    top:1.5rem;
    left:-.35rem;
    width:.9rem;
    height:.9rem;
    border-radius:50%;
    border:.2rem solid rgba(255,255,255,0.05);
    box-shadow:0px 0px 50px rgba(255,255,255,0);
  }
  input[type="checkbox"]:checked ~ .lamp:before {
    background:rgba(255,255,255,1);
    box-shadow:0px 2px 10px rgba(255,255,255,0.8),
    0px 5px 50px rgba(255,255,255,0.8),
    0px 8px 80px rgba(255,255,255,0.6);
  }
  html, body {
    background: #fff0cc;
    width:100%;
    height:100%;
    font-family: 'visionregular', sans-serif;
  }

  .nom_lampe{
    background: #fafafa;
    color: #272f3b;
    position: absolute;
    bottom: 0;
    width: 100%;
    border-radius: 0 0 20px 20px;
    height: 50px;
    box-shadow: 0 -10px 5px -5px #333;
  }

  .nom_lampe > .container > h4 {
    position: absolute;
    margin-left: 20px;
    display: block;
    width: 70px;
    bottom:20px;
    left:20px;
  }
  .nom_lampe > .container > .icon {
    position: absolute;
    height: 33px;
    width: 40px;
    bottom: 10px;
    right: 20px;
    font-size: 25px;
    border:none;
    background: none;
  }
  .nom_lampe > .container > .icon:focus , .nom_lampe > .container > .icon:active{
    border:none;
  }

  .dialog {
    text-align: center;
    position: absolute;
    top:0;
    width: 100%;
    height: 100%;
    background: white;
    margin-top: 45px;
    border:2px solid black;
  }


  .rotateZ-enter-active {
    transition: all .8s ease;
  }
  .rotateZ-leave-active {
    transition: all .8s ease;
  }
  .rotateZ-enter {
    transform: rotateY( 180deg );
  }
  .rotateZ-leave-to {
    transform: translateX( -100% ) rotateY( -180deg );
  }

  .destroy-leave-active {
    transition: all .5s ease;
  }
  .destroy-leave-to {
    transform: translateX(-150%);
    opacity: 0;
  }

  .params{
    text-align: center;
    margin-top: 20px;
  }

  .icon-bs{
    font-size : 45px!important;
    margin-top: 20px;
  }

  .icon-add {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    margin: 0;
    color: #fafafa;
    font-size: 4em !important;
  }

  .button {
    border:none;
    padding:5px 15px;
    border-radius:5px;
    background:#5cb85c;
    font:bold 13px Arial;
    color:#fff;
    margin-top: 15px;
    width:90%;
  }

  .params button{
    background:#5cb85c;
  }

  .suppression button{
    background:#d34836;
  }

  .inputs input:first-child{
    margin:0;
  }

  .inputs input {
    width: 90%;
    padding: 8px 5px;
    margin: 6px 0 0 0;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }

  @media (max-width: 380px){
    .contenant{
      width:92%;
      height: 220px;
    }
  }
  @media (min-width: 380px) and (max-width : 1025px){
    .contenant{
      width: 235px;
      height: 230px;
    }
  }
  @media (min-width : 1025px){
    .contenant{
      width:180px;
      height: 220px;
    }
  }
</style>
