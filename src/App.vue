<template>
  <div id="app">
      <header class="header blue-section"><h1>Random Gif Cat</h1></header>
      <section class="section_personalization salmon-section">
        <div class="section_personalization__item"><div class="item_title"><p>Título:</p></div><div class="item_selector"><input v-model="title"/></div></div>
        <div class="section_personalization__item"><div class="item_title"><p>Filtro:</p></div><div class="item_selector"><select v-model="selectedFiltro"><option v-for="(filter,index) in filters" :key="index" :value="filter">{{filter}}</option></select></div></div>
        <div class="section_personalization__item"><div class="item_title"><p>Color:</p></div><div class="item_selector"><select v-model="selectedColor"><option v-for="(color,index) in colors" :key="index" :value="color">{{color | colorTranslation}}</option></select><div :style="{backgroundColor : selectedColor}" class="color_palet"></div></div></div>
        <div class="section_personalization__item"><div class="item_title"><p>Tamaño:</p></div><div class="item_selector"><input v-model.number ="size" type="number"/></div></div>
      </section>
      <section class="section_mycat blue-section">
        <button :disabled="disableButton" @click="onGetMyCat">Obtener mi gato</button>
        <img :src="mycat">
      </section>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      title: '',
      size: '',
      selectedFiltro: 'sepia',
      selectedColor: 'red',
      filters : ['blur','mono','sepia','negative','paint','pixel'],
      colors: ['green','blue','red','yellow','brown'],
      mycat : 0,
      disableButton: false
    }
  },
  methods: {
    async onGetMyCat() {
      try{
        this.mycat = '';
        this.disableButton = true;
      const url = `https://cataas.com/cat/gif/says/${this.title}?filter=${this.selectedFiltro}&color=${this.selectedColor}&size=${this.size}`;
      await axios.get(url);
      this.mycat = url;
      this.disableButton = false;
      }catch(Error){
        console.log(Error);
        alert("Hubo un problema mientras se obtenía a tu gato: grrrr :(");
        this.disableButton = false;
      }
    }
  },
  filters: {
    colorTranslation: function(selectedColor) {
      let spanishColor = '';
      switch(selectedColor){
        case 'red' : spanishColor = 'rojo';break;
        case 'yellow': spanishColor = 'amarillo';break;
        case 'brown' : spanishColor = 'carmelita';break;
        case 'blue' : spanishColor = 'azul'; break;
        case 'green': spanishColor = 'verde'; break;
        default: spanishColor = this.selectedColor;
      }
      return spanishColor;
    }
  },
  computed: {
    base64ImgComputed() {
      return `data:image/gif;base64,${this.mycat}`;
    }
  },
}
</script>

<style lang="scss">
#app {
  box-sizing: border-box;
}

.header {
  height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.blue-section{
  background-color: #AEC6CF;
}

.salmon-section{
  background-color: #ff8c69;
}

.section{

  &_personalization{

    padding: 5px 0;
  
  &__item {
  display: flex;

  .item{

    &_title{
      width: 50%;
      height: 50px;
      display: flex;
      align-items: center;
      margin-right: 5px;
      flex-wrap: wrap;

      > p{
        flex-basis: 100%;
        text-align: right;
      }
    }

    &_selector{
      width: 50%;
      height: 50px;
      display: flex;
      align-items: center;
      justify-content: right;
      margin-left: 5px;
    }
  }
  


  input,select{
    width: 200px;
    height: 25px;
  }

  .color_palet{
    width: 25px;
    height: 25px;
    clip-path: circle(50%);
    display: inline-block;
    margin-left: 5px;
  }
  }

}
  
  &_mycat {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;

    *{
      margin: 10px 0;
    }
  }

}


</style>
