<template>
  <div class='params'>
    <div class='inputs'>
      <input type='text' name='newName' v-model='newName' placeholder='Nom'>
      <input type='text' name='pgpio' v-model='pgpio' placeholder='Port GPIO'>
      <button class="button" @click='addLamp()'>Ajouter une lampe</button>
    </div>
  </div>
</template>

<script>
  export default {
    props: {},
    data() {
      return {
        newName: '',
        pgpio: ''
      }
    },
    methods: {
      addLamp() {
        this.$http.post('http://149.202.41.94:8080/add', {'name': this.newName, 'pgpio': this.pgpio}).then(
          response => {
            this.result = response.body;
            this.newName = '';
            this.pgpio = '';
            this.$http.post('http://149.202.41.94:8080/select', {'id': response.body.results.insertId}).then(
              response => {
                this.$router.push('/');
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
  .params {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .params button{
    background:#5cb85c;
  }

  .inputs {
    max-width: 380px;
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
</style>
