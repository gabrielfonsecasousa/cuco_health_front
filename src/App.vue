<template>
  <div class="container">
    <h1>Lista de Clientes</h1>
    <table class="table mt-2">
      <thead>
        <tr>
          <th>Nome</th>
          <th>CPF</th>
          <th>Data de Nascimento</th>
          <th>Telefone</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>test</td>
          <td>teste</td>
          <td>test</td>
          <td>test</td>
          <td>
            <div class="btn-group mr-2">
              <button
                class="btn btn-success"
                data-toggle="modal"
                data-target="#exampleModalSubmit"
              >
                Editar
              </button>
            </div>
            <div class="btn-group mr-2">
              <button
                class="btn btn-danger"
                data-toggle="modal"
                data-target="#exampleModal"
              >
                Deletar
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
