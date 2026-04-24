<script setup lang="ts">
import { ref } from 'vue';

const iIngresso = ref(0);
const iIngressosGeral = ref(0);
const iValorGeral = ref(0);
const iIngressosDinheiro = ref(0);
const iValorDinheiro = ref(0);
const iIngressosCartao = ref(0);
const iValorCartao = ref(0);
const iValorIngresso = ref(25);

function adicionar(): void {
   if(++iIngresso.value >= 10) {
      iIngresso.value = 10;
   }   
}

function remover(): void {   
   if(--iIngresso.value <= 0) {
      iIngresso.value = 0;
   }   
}

function adicionarVenda(bDinheiro: boolean) {
   iIngressosGeral.value += iIngresso.value;
   iValorGeral.value += iIngresso.value * iValorIngresso.value;
   
   if(bDinheiro) {
      iValorDinheiro.value += iIngresso.value * iValorIngresso.value;
      iIngressosDinheiro.value += iIngresso.value;
      return iIngresso.value = 0;
   }
   
   iValorCartao.value += iIngresso.value * iValorIngresso.value;
   iIngressosCartao.value += iIngresso.value;
   iIngresso.value = 0;
}
</script>

<template>
  <div class="container">
      <div class="caixa card">         
         <h4>Ingressos Vendidos</h4>
         <p>Quantidade: <span>{{ iIngressosGeral }}</span></p>
         <p>Total: <span>R$ {{ iValorGeral.toFixed(2) }}</span></p>
         <div class="card" id="ingressosDinheiro">
            <h4>Dinheiro</h4>
            <p>Quantidade: <span>{{ iIngressosDinheiro }}</span></p>
            <p>Total: <span>R$ {{ iValorDinheiro.toFixed(2) }}</span></p>
         </div>
         <div class="card" id="ingressosCartao">
            <h4>Cartão</h4>
            <p>Quantidade: <span>{{ iIngressosCartao }}</span></p>
            <p>Total: <span>R$ {{ iValorCartao.toFixed(2) }}</span></p>
         </div>
      </div>
      <div class="quadroValores">
         <input type="radio" name="valor" value="25" id="normal" v-model="iValorIngresso">
         <label for="normal">R$ 25,00</label>
         <input type="radio" name="valor" value="20" id="reduzido" v-model="iValorIngresso">
         <label for="reduzido">R$ 20,00</label>
      </div>
      <div class="acoes">
         <div class="botao">
            <button @click="remover()"><i class="fa fa-minus"></i></button>
            <input readonly max="10" type="number" name="numeroIngressos" id="numeroIngressos" :value="iIngresso">
            <button @click="adicionar()"><i class="fa fa-plus"></i></button>      
         </div>
         <div class="tipo">
            <button @click=adicionarVenda(true)>Dinheiro</button>
            <button @click=adicionarVenda(false)>Cartão</button>
         </div>
      </div>
  </div>
</template>

<style scoped>
.container {
   width: 90%;
   margin: 10% auto;
}

.card {
   font-weight: bold;
   border-radius: 10px;
   width: 100%;
   height: 100%;
   color: white;
   padding: 5px;
}

.caixa {
   display: flex;
   flex-direction: column;
   background-color: rgba(255, 1, 1, 0.956);
   padding: 10px;
   color: black !important;
   margin-bottom: 20%;
}

#ingressosDinheiro {
   background-color: rgb(0, 151, 0);
   margin: 5px 0;
}

#ingressosCartao {
   background-color: rgb(0, 60, 255);
}

.quadroValores {
   display: flex;   
   justify-content: center;
   margin-bottom: 10%;
   gap: 10px;
   color: white;
   font-weight: bold;
   font-size: 1.3rem;
}

.quadroValores input {
   display: none;   
}

.quadroValores label {    
   background-color: gray;
   padding: 15px 5px;
   border-radius: 5px;
}

.quadroValores input:checked + label {
   background-color: rgb(0, 151, 0);
}

.acoes {
   display: flex;
   align-items: center;
   flex-direction: column;     
   margin-bottom: 20%;
}

.botao {
   gap: 25px; 
   width: 100%;
   display: flex;
   justify-content: center;
   margin-bottom: 30%;
}

.botao button {
   padding: 20px;
   border-radius: 60px;
   border: 1px solid white;
   outline: none;   
   background-color: rgb(90, 90, 90);
   color: white;
   font-size: 3rem;
}

.botao input {
   width: 25%;
   font-size: 4rem;
   background-color: transparent;
   border: none;
   outline: none;
   text-align: center;
   color: white;   
}

.tipo {
   display: flex;
   justify-content: center;
   gap: 20px;
   width: 100%;
}

.tipo button {
   background-color: rgb(0, 140, 255);
   border: none;
   border-radius: 5px;
   padding: 10px;
   color: white;
   font-size: 1.5rem;
   font-weight: bold;
   width: 100%;
}
</style>