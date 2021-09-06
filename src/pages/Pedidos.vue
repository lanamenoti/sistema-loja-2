<template>
  <div class="pedidos">
    <Header></Header>
    <h2>Todos os pedidos</h2>
    <div v-for="pedido in pedidos" :key="pedido._id">
      <CardPedido
        :id="pedido.produtoId"
        :cpfCliente="pedido.clienteCPF"
        :valorTotal="pedido.ValorTotal.toString()"
        :valorUnitario="pedido.valorUnitario.toString()"
        :quantidade="pedido.quantidade.toString()"
      />
    </div>
    <Footer></Footer>
  </div>
</template>
<script>
import Header from "../components/Header.vue"
import Footer from "../components/Footer.vue"
import CardPedido from "../components/CardPedido.vue"

export default {
  name: "Pedidos",
  components: {
    Header,
    Footer,
    CardPedido,
  },
  data: function() {
    return {
      pedidos: [],
    }
  },
  methods: {
    getPedidos: async function() {
      const result = await fetch("http://localhost:3000/pedidos")
        .then((res) => res.json())
        .catch((erro) => {
          return {
            erro: true,
            message: erro,
          }
        })

      if (!result.erro) {
        this.pedidos = result
      }
    },
  },
  created: function() {
    this.getPedidos()
  },
}
</script>
<style>
.pedidos h2 {
  margin-left: 200px;
}
</style>
