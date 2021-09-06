<template>
  <section class="pedido">
    <div class="container">
      <div class="row">
        <div class="col-md-12 pedido__info">
          <p>Código: {{ id }}</p>
          <p>CPF Cliente: {{ cpfCliente }}</p>
          <p>Valor Unitário: {{ valorUnitario }}</p>
          <p>Valor Total: {{ valorTotal }}</p>
          <p>Quantidade: {{ quantidade }}</p>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
export default {
  name: "CardPedido",
  props: {
    id: String,
    cpfCliente: String,
    valorUnitario: String,
    valorTotal: String,
    quantidade: String,
  },
}
</script>
<style>
.pedido {
  margin: 10px;
}

.pedido__info {
  padding: 5px;
  border: 1px solid rgb(122, 117, 117);
  border-radius: 5px;
}

.pedido p {
  margin: 3px;
  font-size: 14px;
  color: rgb(70, 66, 66);
}
</style>
