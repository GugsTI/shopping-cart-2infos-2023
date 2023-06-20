<script setup>
import { ref } from 'vue'
import { livros } from '@/_data/livros.js'

const carrinho = ref({
  itens: [],
  total: 0
})

function atualizaQuantidadeItem(item) {
  carrinho.value.total -= item.total
  item.total = item.price * item.quantidade
  carrinho.value.total += item.total
}

function removerItemCarrinho(item) {
  const index = carrinho.value.itens.findIndex((i) => i.id === item.id)
  carrinho.value.total -= item.total
  carrinho.value.itens.splice(index, 1)
}

function adicionarAoCarrinho(livro) {
  const index = carrinho.value.itens.findIndex((item) => item.id === livro.id)
  if (index === -1) {
    carrinho.value.itens.push({
      ...livro,
      quantidade: 1,
      total: livro.price
    })
    carrinho.value.total += livro.price
  } else {
    carrinho.value.itens[index].quantidade++
    carrinho.value.itens[index].total += livro.price
    carrinho.value.total += livro.price
  }
}

function formatarPreco(preco) {
  return 'R$ ' + preco.toFixed(2).replace('.', ',')
}
</script>

<template>
  <div class="container-fluid p-5 bg-warning p-3 text-white text-center">
    <h1>Binco Livrary</h1>
  </div>
  <div class="container-geral">
    <div class="listagem-livros">
      <div  class=" bg-warning card-livro"  v-for="livro in livros" :key="livro.id">
        <div class="wrap-livro">
          <img :src="livro.img" alt="Capa do livro" class="capa-livro" />
        </div>
        <p class="titulo-livro">{{ livro.title }}</p>
        <p class="autor-livro">{{ livro.author }}</p>
        <p class="preco-livro">{{ formatarPreco(livro.price) }}</p>
        <button class="btn btn-dark" @click="adicionarAoCarrinho(livro)">Adicionar ao carrinho</button>
      </div>
    </div>

    <div class="carrinho">
      <br>

      <button type="button" class="btn btn-outline-warning" data-bs-toggle="collapse" data-bs-target="#demo">carrinho</button>
      <div id="demo" class="carrinho">
        <h2 style="color:gold;">Meu carrinho</h2>
        <p v-if="carrinho.itens.length === 0">Seu carrinho está vazio</p>
        <div v-else>
          <div class="item-carrinho" v-for="(item, index) in carrinho.itens" :key="index">
            <div class="info-livro">
              <div class="imagem-livro">
                <img :src="item.img" class="icon-capa-livro" />
              </div>
              <div class="detalhes-livro">
                <p>{{ item.title }}</p>
                <p class="info-livro-preco">{{ formatarPreco(item.price) }}/un</p>
              </div>
              <div>
                Qtde:
                <input class="input" type="number" v-model="item.quantidade" @change="atualizaQuantidadeItem(item)" min="1" />
                <button class="arredondamento" @click="removerItemCarrinho(item)">&#128465;</button>
                <p>Total: {{ formatarPreco(item.total) }}</p>
                <button  type="button" class="btn btn-outline-primary" ><a href="https://www.amazon.com.br/cart?ref_=sw_gtc">finalizar compra</a></button>

              </div>
            </div>
          </div>
        </div>
      </div>
      <p>Total: {{ formatarPreco(carrinho.total) }}</p>
    </div>
  </div>

  <div>
  </div>
</template>

<style scoped>

.arredondamento{
  border-radius: 10px;
  background-color: rgb(255, 253, 253);
}

.input{
  border-radius: 5px;
  background-color: rgb(255, 255, 255);


}
.detalhes-livro input[type='number'] {
  width: 50px;
  text-align: center;
  border: none;
  border-bottom: 1px solid black;
  background-color: transparent;
  margin-left: 10px;
}

.detalhes-livro button {
  background-color: transparent;
  border: none;
  cursor: pointer;
  font-size: 1.5rem;
  color: black;
  padding: 0;
  margin: 0;
}

.info-livro {
  display: flex;
  margin-bottom: 10px;
}

.detalhes-livro {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.detalhes-livro p {
  margin: 0;
}

.detalhes-livro div {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.info-livro-preco {
  margin-left: auto;
}

.icon-capa-livro {
  width: 30px;
  margin-right: 10px;
}

.container-geral {
  display: grid;
  grid-template-columns: 3fr 1fr;
}

.carrinho {
  min-width: 20%;
  font-family: 'Times New Roman', Times, serif;
  color: aliceblue;


}

.listagem-livros {
  display: flex;
  flex-wrap: wrap;
}

.card-livro {
  margin: 5px 10px;
  padding: 10px;
  border-radius: 10px;
  width: 180px;
}

.wrap-livro {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgb(255, 217, 0);
  border-radius: 10px;

  height: 270px;
}

.capa-livro {
  width: 90%;
  max-height: 100%;
}

.card-livro p {
  margin: 0;
}

.card-livro .titulo-livro {
  font-weight: bold;
  margin-bottom: 5px;
}
</style>
