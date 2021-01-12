<template>
  <div class="eoq">
    <nav id="titleAppCont"><h1 id="titleApp">EOQ App</h1></nav>
    <div id="contenidoApp">
        <div id="cardIn">
            <h2 id="titleIn">Ingresa los datos</h2>
            <div id="contIn">
                <div class="inputs">
                    <p>Costo de compra (C)</p>
                    <button class="btn btn-success" @click="in1=!in1" v-if="!this.in1">+</button>
                    <button class="btn btn-danger" @click="in1=!in1" v-if="this.in1">-</button>
                    <input type="number" step="any" v-if="this.in1" v-model="C">
                </div>
                <div class="inputs">
                    <p>Demanda por unidad de tiempo (D)</p>
                    <button class="btn btn-success" @click="in2=!in2" v-if="!this.in2">+</button>
                    <button class="btn btn-danger" @click="in2=!in2" v-if="this.in2">-</button>
                    <input type="number" step="any" v-if="this.in2" v-model="D">
                </div>
                <div class="inputs">
                    <p>Costo por ordenar el producto (Co)</p>
                    <button class="btn btn-success" @click="in3=!in3" v-if="!this.in3">+</button>
                    <button class="btn btn-danger" @click="in3=!in3" v-if="this.in3">-</button>
                    <input type="number" step="any" v-if="this.in3" v-model="Co">
                </div>
                <div class="inputs">
                    <p>Costo por mantener el inventario (Ch)</p>
                    <button class="btn btn-success" @click="in4=!in4" v-if="!this.in4">+</button>
                    <button class="btn btn-danger" @click="in4=!in4" v-if="this.in4">-</button>
                    <input type="number" step="any" v-if="this.in4" v-model="Ch">
                </div>
                <div class="inputs">
                    <p>Tiempo de entrega en días (L)</p>
                    <button class="btn btn-success" @click="in5=!in5" v-if="!this.in5">+</button>
                    <button class="btn btn-danger" @click="in5=!in5" v-if="this.in5">-</button>
                    <input type="number" step="any" v-if="this.in5" v-model="L">
                </div>
                <div class="inputs">
                    <p>Cantidad de dias de trabajo al año</p>
                    <button class="btn btn-success" @click="in6=!in6" v-if="!this.in6">+</button>
                    <button class="btn btn-danger" @click="in6=!in6" v-if="this.in6">-</button>
                    <input type="number" step="any" v-if="this.in6" v-model="dias">
                </div>
            </div>
        </div>
        <div id="cardOut">
            <h2 id="titleOut">Resultados</h2>
            <div id="contOut">
                <div class="resultados">
                    <p>Cantidad optima a pedir (Q): <span>{{getQ}}</span></p>
                </div>
                <div class="resultados">
                    <p>Punto de reorden (R): <span>{{getR}}</span></p>
                </div>
                <div class="resultados">
                    <p>Costo anual de inventario: <span>{{getCAI}}</span></p>
                </div>
                <div class="resultados">
                    <p>Numero de ordenes: <span>{{getN}}</span></p>
                </div>
                <div class="resultados">
                    <p>Dias entre ordenes: <span>{{getDEO}}</span></p>
                </div>
                 <div class="resultados">
                    <p>Tiempo de entrega en dias (L): <span>{{getL}}</span></p>
                </div>
                <div class="resultados">
                    <p>Costo Anual de Ordenar: <span>{{getCAO}}</span></p>
                </div>
                <div class="resultados">
                    <p>Costo Anual de Mantener: <span>{{getCAM}}</span></p>
                </div>
                <div class="resultados">
                    <p>Costo Total: <span>{{getCT}}</span></p>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EOQ',
  props: {
    msg: String
  },
  data() {
      return {
          in1: false,
          in2: false,
          in3: false,
          in4: false,
          in5: false,
          in6: false,
        C: NaN,
        D: NaN,
        Co: NaN,
        Ch: NaN,
        L: NaN,
        dias: 365,
        R: NaN,
        Q: NaN,
        N: NaN,
        Cao: NaN,
        Cai: NaN,
        Deo: NaN,
        Cam: NaN,
        CT: NaN
      }
  },
  computed: {
    setInicializar(){
        if(this.in1==false){
            this.C = NaN; 
        }
        if(this.in2==false){
            this.D = NaN; 
        }
        if(this.in3==false){
            this.Co = NaN; 
        }
        if(this.in4==false){
            this.Ch = NaN; 
        }
        if(this.in5==false){
            this.L = NaN;
        }
        if(this.in6==false){
            this.dias = 365;
        }
    },
    getQ(){
        //Cantidad optima a ordenar
        return this.Q = Math.sqrt((2*this.D*this.Co)/this.Ch)
      },
      getN(){
        //Numero de ordenes por año
        return this.N = (this.D/this.Q)
      },
      getL(){
          //Tiempo de entrega en dias
          if(this.in6==false){
            this.dias = 365; 
          }   
          if(this.in5==false){
              return this.L = (this.dias/this.N)
          }else{
              return this.L
          }    
      },
      getR(){
          //Punto de reorden
          if(this.in6==false){
            this.dias = 365; 
          }
          return this.R = (this.D*this.L)/this.dias
      },
      getCAO(){
          //Costo Anual de Ordenar
          return this.Cao = (this.D/this.Q)*this.Co
      },
      getCAM(){
          //Costo Anual de Mantener
          return this.Cam = (this.Q/2)*this.Ch
      },
      getCAI(){
          //Costo Anual de Inventario
          return this.Cai = ((this.D/this.Q)*this.Co)+((this.Q/2)*this.Co)
      },
      getDEO(){
          //Dias entre ordenes
          return this.Deo = 365/this.N
      }, 
      getCT(){
        //Costo total
        return this.CT = ((this.D*this.C)+((this.D/this.Q)*this.Co)+((this.Q/2)*this.Ch));
      },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .btn-danger{
        background-color: #821963;
    }
    .inputs{
        padding: 13px;
    }

    input{
        background-color: #121525;
        color: #fff;
        text-align: center;
        border-color: #c80058; 
    }
    .eoq{
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        flex-wrap: nowrap;
    }

    #titleAppCont{
        width: 100%;
        height: 15%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        flex-wrap: nowrap;
    }

    #titleApp{
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        flex-wrap: nowrap;
    }   

    #contenidoApp{
        width: 100%;
        height: 85%;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        flex-wrap: nowrap;
    }

    #cardIn, #cardOut{
        width: 85%;
        height: 45%;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        flex-wrap: nowrap;
        background-color: #FC122C;

        border-radius: 10px 10px 10px 10px;
        -moz-border-radius: 10px 10px 10px 10px;
        -webkit-border-radius: 10px 10px 10px 10px;
        box-shadow: 4px 4px 5px 0px rgba(200,0,80,1);
        transition: all .3s ease-in-out;
    }
    #cardIn:hover, #cardOut:hover{
        box-shadow: -3px 18px 20px 0px rgba(61,34,80,1);
    }
    #titleIn, #titleOut{
        width: 100%;
        height: 10%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        flex-wrap: nowrap;
        padding-top: 15px;
        margin-bottom: 15px;
    }

    #contIn{
        width: 100%;
        height: 90%;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
        align-items: center;
        flex-wrap: wrap;
    }

    #contOut{
        width: 100%;
        height: 90%;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
        align-items: center;
        flex-wrap: wrap;
    }

    span{
        color:#121525;
    }

    
    /* Extra small devices (phones, 600px and down) */
    @media only screen and (max-width: 999px) {
        .eoq{
            justify-content: none;
            height: auto;
            padding-bottom: 25px; 
        }
        #titleAppCont{
            height: 5%;
        }

        #contenidoApp{
            height: 90%;
            justify-content: center;
        }

        #cardIn, #cardOut{
            height: auto;
        }

        #contIn{
            padding-bottom: 4px;
        }

        #cardOut{
            margin-top: 5%;
        }
    }
</style>