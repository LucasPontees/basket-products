<template>
  <div class="container container-lg">
    <div class="row mt-5">
    <div class="col-md-5">
      <div class="card">
        <div class="card-body">
          <div class="card-header">
            <input type="text" class="w-100" v-model="searchProduct" placeholder="Pesquisar"> 
          </div>
          <div class="container-scroll" style="height: 50vh; overflow-y: auto;">
            <table class="table table-bordered">
              <thead>
                <tr>
                  <th class="text-start" style="width: 90%;">Descrição</th>
                  <th class="text-center" style="width: 10%;"><i class="bi bi-file-arrow-down-fill"></i></th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(produto, index) in this.produtos" :key="index">
                  <td class="text-start">{{ produto.title }} - R$ {{ produto.price }}</td>
                  <td class="text-center">
                    <button 
                      v-if="!isInCarrinho(produto)"
                      class="btn btn-success" 
                      @click="this.addProdutoCarrinho(produto)"
                    ><i class="bi bi-plus-circle"></i></button>
                    <button 
                      v-else 
                      class="btn btn-danger" 
                      @click="this.$store.dispatch('removerProdutoCarrinho', produto.id)"
                    ><i class="bi bi-trash2-fill"></i></button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <div class="col-md-7">
      <div class="card">
        <div class="card-body">
          <div class="container-scroll" style="height: 50vh; overflow-y: auto;">
            <table class="carrinho table table-bordered">
              <thead>
                <tr>
                  <th class="text-start" style="width: 90%;">Descrição</th>
                  <th class="text-center" style="width: 10%;">Excluir</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(produto, index) in produtosNoCarrinho" :key="index">
                  <td class="text-start">{{ produto.title }}</td>
                  <td class="text-center">
                    <button class="btn btn-danger" @click="this.$store.dispatch('removerProdutoCarrinho', produto.id)"><i
                        class="bi bi-trash align-middle"></i></button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <div class="card-footer">
          <h3>Valor total: R$ {{ this.valorTotal() }}</h3>
        </div>
      </div>
    </div>
  </div>
  </div>

</template>

<script>
import { mapState } from 'vuex'

export default {
  name: 'HomeView',
  data() {
    return {
      searchProduct: ''
    }
  },
  computed: {
    searchProducts() {
      return this.produtos.filter(produto => {
        return produto.title.toLowerCase().includes(this.searchProduct.toLowerCase())
      })
    },
    ...mapState([
      'produtos',
      'produtosNoCarrinho',
    ])
  },
  methods: {
    addProdutoCarrinho(produto) {
      produto.qtd = 1;
      this.$store.dispatch('addProdutoCarrinho', produto);
    },
    isInCarrinho(produto) {
      return this.produtosNoCarrinho.find(item => item.id == produto.id)
    },
    diminuirQtdProduto(produto) {
      if (produto.qtd > 1) {
        produto.qtd--;
      }
    },
    valorTotal() {
      let total = 0;
      this.produtosNoCarrinho.forEach(item => {
        total += item.price * item.qtd;
      });
      return total.toFixed(2);
    },
  }
}
</script>

<style lang="scss">

.home {
  padding: 20px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;

  .cardProduto {
    height: auto;
    margin: 0 10px 10px 0;
    padding: 15px;
    background-color: #F5F5F5;
    display: flex;
    flex-direction: column;
    align-items: center;

    img {
      width: 70%;
      height: auto;
      object-fit: cover;
    }

    div {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

  
  }
}

</style>
