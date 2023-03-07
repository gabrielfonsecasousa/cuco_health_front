<template>
  <div class="container">
    <h1>Lista de Clientes</h1>
    <div class="row">
      <div class="col-3 form-group mr-3">
        <input
          type="text"
          class="form-control"
          placeholder="Nome"
          v-model="nameFilter"
        />
      </div>
      <div class="col-3 form-group mr-3">
        <input
          type="text"
          class="form-control"
          placeholder="CPF"
          v-model="cpfFilter"
          v-mask="'##.###.###/####-##'"
        />
      </div>

      <div class="form-group">
        <button class="btn btn-primary" @click="filterCustomers()">
          Filtrar
        </button>
      </div>
    </div>
    <div class="row">
      <div class="col-12" style="text-align: left">
        <button
          class="btn btn-primary"
          data-toggle="modal"
          data-target="#exampleModalSubmit"
          @click="showCustomer(null)"
        >
          Criar Cliente
        </button>
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
        <tr v-for="(customer, index) in customers" :key="index">
          <td>{{ customer.name }}</td>
          <td>{{ customer.cpf }}</td>
          <td>{{ this.dateFormat(customer.birthday) }}</td>
          <td>{{ customer.phone }}</td>
          <td>
            <div class="btn-group mr-2">
              <button
                class="btn btn-success"
                data-toggle="modal"
                data-target="#exampleModalSubmit"
                @click="showCustomer(customer)"
              >
                Editar
              </button>
            </div>
            <div class="btn-group mr-2">
              <button
                class="btn btn-danger"
                data-toggle="modal"
                data-target="#exampleModal"
                @click="showCustomer(customer)"
              >
                Deletar
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
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
              {{ message }}
            </div>
            <div class="form-group">
              <label for="name">Name</label>
              <input
                type="text"
                class="form-control"
                id="name"
                required
                v-model="form.name"
              />
            </div>
            <div class="form-group">
              <label for="cpf">CPF</label>
              <input
                type="text"
                class="form-control"
                required
                :onkeyup="maskCpf"
                id="cpf"
                min="12"
                v-model="form.cpf"
              />
            </div>
            <div class="form-group">
              <label for="birthday">Aniversario</label>
              <input
                type="date"
                required
                class="form-control"
                id="birthday"
                v-model="form.birthday"
              />
            </div>
            <div class="form-group">
              <label for="phone">Phone</label>
              <input
                type="text"
                class="form-control"
                id="phone"
                :onkeyup="maskPhone"
                v-model="form.phone"
              />
            </div>
            <div class="form-group" style="text-align: right">
              <button
                type="submit"
                class="btn btn-success"
                style="text-align: right"
              >
                {{ form.id ? "Editar" : "Criar" }}
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
  <!-- Delete -->
  <div
    v-if="customer"
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
            Deletar Cliente {{ customer.name }}
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
          Voce tem certesa que deseja deletar o cliente: {{ customer.name }}
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-danger" data-dismiss="modal">
            Fechar
          </button>
          <button
            type="button"
            data-dismiss="modal"
            @click="deleteCustomer(customer)"
            class="btn btn-success"
          >
            Sim
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/javascript">
import axios from "axios";

export default {
  data() {
    return {
      customers: [],
      customer: Object,
      selectedCustomer: null,
      message: "",
      nameFilter: "",
      cpfFilter: "",
      showForm: false,
      form: Object,
    };
  },
  mounted() {
    this.getCustomers();
  },
  methods: {
    maskCpf(event) {
      const cpf = event.target.value
        .replace(/\D/g, "") // remove caracteres não numéricos
        .replace(/(\d{3})(\d)/, "$1.$2") // coloca um ponto entre o terceiro e o quarto dígitos
        .replace(/(\d{3})(\d)/, "$1.$2") // coloca um ponto entre o sexto e o sétimo dígitos
        .replace(/(\d{3})(\d{1,2})/, "$1-$2"); // coloca um hífen entre o nono e o décimo dígitos (se houver)
      event.target.value = cpf;
    },
    maskPhone(event) {
      let phone = event.target.value.replace(/\D/g, ""); // remove all non-numeric characters
      phone = phone.replace(/^(\d{2})(\d)/g, "($1) $2"); // add parentheses and space after the first two digits
      phone = phone.replace(/(\d{4})(\d)/, "$1-$2"); // add hyphen after the next four digits
      event.target.value = phone;
    },
    dateFormat(date) {
      let dateString = date;
      let dateObject = new Date(dateString);
      let day = dateObject.getDate();
      let month = dateObject.getMonth() + 1;
      let year = dateObject.getFullYear();
      let formattedDate = `${day.toString().padStart(2, "0")}/${month
        .toString()
        .padStart(2, "0")}/${year.toString()}`;
      return formattedDate;
    },
    dateFormatInput(date) {
      let dateString = date;
      let dateObject = new Date(dateString);
      let year = dateObject.getFullYear();
      let month = (dateObject.getMonth() + 1).toString().padStart(2, "0");
      let day = dateObject.getDate().toString().padStart(2, "0");
      let formattedDate = `${year}-${month}-${day}`;
      return formattedDate;
    },
    filterCustomers() {
      axios
        .get(
          `http://127.0.0.1:8000/api/customers?cpf=${this.cpfFilter}&name=${this.nameFilter}`
        )
        .then((response) => {
          this.customers = response.data["data"];
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getCustomers() {
      axios
        .get("http://127.0.0.1:8000/api/customers")
        .then((response) => {
          this.customers = response.data["data"];
        })
        .catch((error) => {
          console.log(error);
        });
    },
    deleteCustomer(customer) {
      axios
        .delete("http://127.0.0.1:8000/api/customers/" + customer.id)
        .then(() => {
          this.getCustomers();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    showCustomer(customer) {
      var customerObject = {
        id: customer ? customer.id : "",
        cpf: customer ? customer.cpf : "",
        name: customer ? customer.name : "",
        birthday: customer ? this.dateFormatInput(customer.birthday) : "",
        phone: customer ? customer.phone : "",
      };
      this.customer = customerObject;
      this.form = customerObject;
    },
    onDelete(customer) {
      axios
        .delete("http://127.0.0.1:8000/api/customers/" + customer.id)
        .then(() => {
          this.getCustomers();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    async handleSubmit() {
      try {
        this.form.id
          ? await axios.put(
              `http://127.0.0.1:8000/api/customers/${this.form.id}`,
              this.form
            )
          : await axios.post("http://127.0.0.1:8000/api/customers", this.form);
        this.getCustomers();
        const closeModal = document.querySelector("#closeModal");
        closeModal.click();
        this.message = "";
      } catch (error) {
        this.message = error.response.data.message;
      }
    },
    onCancel() {
      this.showForm = false;
    },
  },
};
</script>
 
