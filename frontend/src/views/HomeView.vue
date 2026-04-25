<script setup lang="ts">
import { onBeforeMount, ref } from 'vue';

const iIngresso = ref(0);
const iIngressosGeral = ref(0);
const iValorGeral = ref(0);
const iIngressosDinheiro = ref(0);
const iValorDinheiro = ref(0);
const iIngressosCartao = ref(0);
const iValorCartao = ref(0);
const iValorIngresso = ref(25);

onBeforeMount(() => {
   getDadosSessao();
})

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
      setDadosSessao();
      return iIngresso.value = 0;
   }
   
   iValorCartao.value += iIngresso.value * iValorIngresso.value;
   iIngressosCartao.value += iIngresso.value;
   iIngresso.value = 0;
   setDadosSessao();
}

function setDadosSessao() {
   window.sessionStorage.setItem('iIngressosGeral', `${iIngressosGeral.value}`);
   window.sessionStorage.setItem('iValorGeral', `${iValorGeral.value}`);
   window.sessionStorage.setItem('iIngressosDinheiro', `${iIngressosDinheiro.value}`);
   window.sessionStorage.setItem('iValorDinheiro', `${iValorDinheiro.value}`);
   window.sessionStorage.setItem('iIngressosCartao', `${iIngressosCartao.value}`);
   window.sessionStorage.setItem('iValorCartao', `${iValorCartao.value}`);   
}

function getDadosSessao() {
   iIngressosGeral.value    = parseInt(window.sessionStorage.getItem('iIngressosGeral')?? '0');
   iValorGeral.value        = parseInt(window.sessionStorage.getItem('iValorGeral')?? '0');
   iIngressosDinheiro.value = parseInt(window.sessionStorage.getItem('iIngressosDinheiro')?? '0');
   iValorDinheiro.value     = parseInt(window.sessionStorage.getItem('iValorDinheiro')?? '0');
   iIngressosCartao.value   = parseInt(window.sessionStorage.getItem('iIngressosCartao')?? '0');
   iValorCartao.value       = parseInt(window.sessionStorage.getItem('iValorCartao')?? '0');   
}
</script>

<template>
  <div class="container">
      <div class="caixa">
         <div class="card" id="ingressosGeral">
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
   margin: 0 auto;
   padding: 10% 0;
   height: 100%;
}

.card {
   display: flex;
   flex-direction: column;
   border-radius: 10px;
   padding: 10px;
   color: white;
   text-shadow: 0px 0px 1px black;   
}

.caixa {
   margin-bottom: 20%;
}

.caixa p {
   font-weight: bold;
}

#ingressosGeral {
   background-color: rgb(105, 105, 105);
   box-shadow: 1px 1px 5px 1px rgb(27, 27, 27);
}

#ingressosDinheiro {
   background-color: rgb(0, 140, 255);
   margin: 5px 0;
}

#ingressosCartao {
   background-color: rgb(0, 140, 255);
}

.quadroValores {
   display: flex;
   width: 100%;
   justify-content: center;
   gap: 10px;
   margin-bottom: 20%;
}

.quadroValores input {
   display: none;   
}

.quadroValores label {    
   border-radius: 5px;
   padding: 22px 5px;
   background-color: rgb(105, 105, 105);
   text-align: center;
   font-weight: bold;
   font-size: 1.3rem;
   color: white;
}

.quadroValores input:checked + label {
   background-color: rgb(0, 140, 255);
   box-shadow: inset 0px 0px 10px 1px black;
}

.acoes {
   display: flex;
   flex-direction: column;
   align-items: center;
   justify-content: center;   
   height: 30%;
}

.botao {
   gap: 25px;
   width: 100%;
   display: flex;
   justify-content: center;
   margin-bottom: 20%;
}

.botao button {
   padding: 20px;
   border-radius: 100px;
   border: 2px solid rgb(255, 255, 255, 0.479);
   outline: none;
   background-color: rgb(51,51,51,0.712);
   color: white;
   font-size: 3rem;
}

.botao button:active {
   box-shadow: inset 0px 0px 10px 1px black;
}

.botao input {
   width: 25%;
   font-size: 4rem;
   background-color: transparent;
   font-weight: bold;
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

.tipo button:active {
   box-shadow: inset 1px 1px 10px 1px black;
}
</style>