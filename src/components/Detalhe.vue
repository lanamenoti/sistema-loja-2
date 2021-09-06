<template>
  <section class="detalhe">
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <h2>Detalhe do Produto</h2>
          <img :src="produto.img" alt="Red dead" />
        </div>
        <div class="col-md-9">
          <h2>{{ produto.title }}</h2>
          <p>R$ {{ produto.price }}</p>
        </div>
        <div class="col-md-3">
          <div class="detalhe__box-price">
            <p>Quantidade</p>
            <input @input="toCalculate" type="number" v-model="quantity" />
          </div>
        </div>
        <div class="col-md-12">
          <hr />
          <h3>Descrição:</h3>
          <p>
            Lorem Ipsum is simply dummy text of the printing and typesetting
            industry. Lorem Ipsum has been the industry's standard dummy text
            ever since the 1500s, when an unknown printer took a galley of type
            and scrambled it to make a type specimen book. It has survived not
            only five centuries, but also the leap into electronic typesetting,
            remaining essentially unchanged. Lorem Ipsum is simply dummy text of
            the printing and typesetting industry. Lorem Ipsum has been the
            industry's standard dummy text ever since the 1500s, when an unknown
            printer took a galley of type and scrambled it to make a type
            specimen book. It has survived not only five centuries, but also the
            leap into electronic typesetting, remaining essentially unchanged
          </p>
          <h4>
            Total : {{ finalQuantity }} * {{ produto.price }} =
            {{ total.toString().replace(".", ",") }}
          </h4>
        </div>
        <div class="col-md-12">
          <button @click="ativarNovoPedido">Fazer Pedido</button>
        </div>
        <div v-if="deveAtivar">
          <NovoPedido
            :valorTotal="total.toString()"
            :valorUnitario="preco.toString()"
            :quantidade="quantity.toString()"
          />
        </div>
      </div>
    </div>
  </section>
</template>
<script>
import NovoPedido from "../components/NovoPedido.vue"

export default {
  name: "Detalhe",
  components: {
    NovoPedido,
  },
  data: function() {
    return {
      deveAtivar: false,
      quantity: 0,
      finalQuantity: 0,
      preco: 0,
      total: 0,
      produto: {},
    }
  },
  methods: {
    toCalculate: function() {
      this.finalQuantity = this.quantity

      if (this.quantity === "") {
        this.finalQuantity = 1
      }

      const total = parseFloat(this.preco) * this.finalQuantity
      this.total = total.toFixed(2)
    },
    getProdutoById: async function() {
      const result = await fetch(
        `http://localhost:3000/produtos/${this.$route.params.id}`
      )
        .then((res) => res.json())
        .catch((erro) => {
          return {
            erro: true,
            message: erro,
          }
        })

      if (!result.erro) {
        this.produto = result
        this.preco = result.price
      }
    },
    ativarNovoPedido: function() {
      this.deveAtivar = !this.deveAtivar
    },
  },
  created: function() {
    this.getProdutoById()
  },
}
</script>
<style>
.detalhe {
  padding: 50px 0px;
}

.detalhe img {
  margin: 20px auto;
  display: block;
}

.detalhe__box-price {
  text-align: right;
  font-weight: bold;
}

.detalhe input {
  width: 50px;
  height: 30px;
  border-radius: 4px;
  padding: 0px 8px;
}

.detalhe button {
  width: 170px;
  height: 40px;
  border-radius: 6px;
  background-color: #ae382b;
  color: #f5a022;
  border: none;
  font-weight: bold;
  display: block;
  margin: 30px auto;
}
</style>
