<template>
  <div id="app">
    <img class="img" src="./assets/cronometro.png">
    <a class="timer">{{numero}}</a>

    <div class="areaBtn">
         <button class="botao" v-on:click="start()">{{botao}}</button>
         <button class="botao" v-on:click="clear(), clearHistory()">CLEAR</button>
    </div>

    <div class="list" v-show="history.length > 0">
        <ul>
          <li v-for="item in history" :key="item">VOCÊ FEZ UMA PAUSA EM: {{ item }}</li>
        </ul>
        <button v-on:click="clearHistory()">Limpar Histórico</button> 
    </div>
     
   
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      numero: 0,
      botao: 'START',
      timer: null,
      ss:0,
      mm: 0,
      hh: 0,
      history: []
    }
  },
  methods: {
    start(){
      if(this.timer !== null){
        //Aqui tem algo rodando no timer...
        clearInterval(this.timer);
        this.timer = null;
        this.botao = "START";

        if(this.ss != 0){
          this.history.push(this.numero);
        }
      } else {
        //O timer está parado ou zerado

        this.timer = setInterval(() => {

          this.rodarTimer();

        }, 100); //1 seg = 1.000 milissegundos
        this.botao = 'PAUSE';
      }
    },
    clear(){

      if(this.timer !== null){
        clearInterval(this.timer);
        this.timer = null;
      }

      this.ss = 0;
      this.mm = 0;
      this.hh = 0;
      this.numero = 0;
      this.botao = 'START'

    },
    rodarTimer(){
        this.ss++;

        if(this.ss == 60){
          //Deu 59 segundos
          this.ss = 0;
          this.mm++;
          
        }
          
          
          if(this.mm == 60){
            //Deu 59 minutos
            this.mm = 0;
            this.hh++;
            
          }

          let format = (this.hh < 10 ? '0'+ this.hh : this.hh) + ':' 
          + (this.mm < 10 ? '0'+ this.mm : this.mm) + ':' 
          + (this.ss < 10 ? '0'+ this.ss : this.ss);
          
          return this.numero = format;
        
      },
      clearHistory(){
        if(this.history.length > 0){
          this.history = [];
        }
      }
  }
 
}
</script>

<style>
  #app{
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .img {
    width: 352px;
    height: 342px;
    padding-top: 50px;  
  }

  .timer{
    color: white;
    font-size: 58px;
    margin-top: -192px ;
  }

  .areaBtn {
    margin-top: 150px;
  }

  .botao {
    -webkit-user-select: none;
    width: 93px;
    background-color: white;
    font-size: 16px;
    border: none;
    border-radius: 5px;
    text-align: center;
    margin-right: 15px;
    margin-left: 15px;
    padding: 6px;
    cursor: pointer;
  }

  .botao:hover {
    opacity: 0.7;
    transition: all 0.50s;
  }

  ul {
    text-align: center;
    padding: 0px;
  }

  ul li {
    margin-top: 4px;
    padding: 15px;
    background-color: rgb(70,70,70);
    list-style: none;
    color: #fff;
    font-size: 18px;
    border-radius: 6px;
  }

  .list button {
    cursor: pointer;
    border: 0;
    background-color: #fff;
    padding: 8px;
    border-radius: 5px;
    margin-bottom: 12px;
  }


</style>
