<template>
  <section class="novoPedido">
    <div class="row" v-if="!pedidoCadastrado">
      <div class="col-md-12">
        <hr />
        <h3>Novo Pedido</h3>
      </div>
      <div class="col-md-6">
        <div class="novoPedido__info__cliente">
          <p>CPF Cliente:</p>
          <div>
            <input type="text" v-model="cpfCliente" id="novoPedido__input" />
            <button @click="getClientePorCpf" id="botao__buscar">Buscar</button>
          </div>
        </div>
      </div>
      <div class="col-md-12" v-if="cliente._id">
        <p>Nome Completo: {{ `${cliente.nome} ${cliente.sobrenome}` }}</p>
        <p>CPF: {{ cliente.CPF }}</p>
        <p>Data de Nascimento: {{ cliente.dataNascimento }}</p>
        <button class="botao__salvar" @click="fazerPedido">
          Salvar Pedido
        </button>
        <hr />
      </div>
    </div>
    <div class="row" v-else>
      <div class="col-md-4">
        <p>Pedido cadastrado com sucesso!</p>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "NovoPedido",
  props: {
    valorTotal: String,
    valorUnitario: String,
    quantidade: String,
  },
  data: function() {
    return {
      cpfCliente: "",
      cliente: {},
      pedidoCadastrado: false,
    }
  },
  methods: {
    getClientePorCpf: async function() {
      const result = await fetch(
        `http://localhost:3000/clientes/busca/${this.cpfCliente}`
      )
        .then((res) => res.json())
        .catch((erro) => {
          console.log(erro)
          return {
            erro: true,
            message: erro,
          }
        })

      if (!result.erro) {
        this.cliente = result
      }
    },
    fazerPedido: async function() {
      const novoPedido = {
        produtoId: this.$route.params.id,
        ValorTotal: this.valorTotal,
        valorUnitario: this.valorUnitario,
        quantidade: this.quantidade,
        clienteCPF: this.cpfCliente,
      }

      const result = await fetch(`http://localhost:3000/pedidos`, {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify(novoPedido),
      })
        .then((res) => res.json())
        .catch((erro) => {
          console.log(erro)
          return {
            erro: true,
            message: erro,
          }
        })

      if (!result.erro) {
        this.pedidoCadastrado = true
      }
    },
  },
}
</script>

<style>
.novoPedido {
  margin: auto 0;
  margin-bottom: 20px;
  /* border-bottom: 1px solid;
  border-top: 1px solid; */
}
.novoPedido__info__cliente {
  display: flex;
  align-items: center;
}
.novoPedido__info__cliente div {
  display: flex;
  align-items: center;
}
#novoPedido__input {
  width: 150px;
  margin-left: 20px;
}
.novoPedido button {
  border-radius: 6px;
  background-color: #ae382b;
  color: #f5a022;
  border: none;
  font-weight: bold;
  display: block;
}
.botao__salvar {
  width: 170px;
  height: 40px;
  margin: 30px auto;
}
#botao__buscar {
  width: 100px;
  height: 35px;
  margin: 0px;
}
</style>
