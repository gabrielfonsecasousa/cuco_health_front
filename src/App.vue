<template>
  <div class="container">
    <h1>Lista de Clientes</h1>
    <div class="row">
      <div class="col-3 form-group mr-3">
        <input type="text" class="form-control" placeholder="Nome" />
      </div>
      <div class="col-3 form-group mr-3">
        <input
          type="text"
          class="form-control"
          placeholder="CPF"
          v-mask="'##.###.###/####-##'"
        />
      </div>

      <div class="form-group">
        <button class="btn btn-primary">Filtrar</button>
      </div>
    </div>
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
    <!-- Create -->
    <div
      class="modal fade"
      id="exampleModalSubmit"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              {{ customer ? "Edição" : "Cadastro" }} de Cliente
            </h5>
            <button
              id="closeModal"
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="handleSubmit" style="text-align: left">
              <div v-if="message" class="alert alert-danger" role="alert">
                teste
              </div>
              <div class="form-group">
                <label for="name">Name</label>
                <input type="text" class="form-control" id="name" required />
              </div>
              <div class="form-group">
                <label for="cpf">CPF</label>
                <input
                  type="text"
                  class="form-control"
                  required
                  id="cpf"
                  min="12"
                />
              </div>
              <div class="form-group">
                <label for="birthday">Aniversario</label>
                <input
                  type="date"
                  required
                  class="form-control"
                  id="birthday"
                />
              </div>
              <div class="form-group">
                <label for="phone">Phone</label>
                <input type="text" class="form-control" id="phone" />
              </div>
              <div class="form-group" style="text-align: right">
                <button
                  type="submit"
                  class="btn btn-success"
                  style="text-align: right"
                >
                  Criar
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <!-- Delete -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              Deletar Cliente cliente
            </h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body" v-if="customer">
            Voce tem certesa que deseja deletar o cliente: cliente
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-danger" data-dismiss="modal">
              Fechar
            </button>
            <button type="button" data-dismiss="modal" class="btn btn-success">
              Sim
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
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
