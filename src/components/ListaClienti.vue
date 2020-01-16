<template>
  <div>
    <el-row :gutter="10">
      <h1>Lista Clienti</h1>
    </el-row>

    <el-row :gutter="10">
      <el-button type="primary" @click="modalInserisciCliente = true"
        >Aggiungi Cliente</el-button
      >
    </el-row>

    <el-dialog :visible.sync="modalInserisciCliente" append-to-body width="50%">
      <el-form label-width="160px" method="POST">
        <el-form-item label="Azienda">
          <el-input v-model="company"></el-input>
        </el-form-item>
        <el-form-item label="Referente">
          <el-input v-model="ref_name"></el-input>
        </el-form-item>
        <el-form-item label="Numero di Telefono">
          <el-input v-model="phone"></el-input>
        </el-form-item>
        <el-form-item label="Indirizzo E-mail">
          <el-input v-model="mail"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="modalInserisciCliente = false">Annulla</el-button>
        <el-button type="primary" @click="inserisciCliente()">Salva</el-button>
      </span>
    </el-dialog>

    <el-dialog :visible.sync="modalModificaCliente" append-to-body width="50%">
      <el-form label-width="160px" method="PATCH">
        <el-form-item label="Azienda">
          <el-input v-model="modCustomer.company"></el-input>
        </el-form-item>
        <el-form-item label="Referente">
          <el-input v-model="modCustomer.ref_name"></el-input>
        </el-form-item>
        <el-form-item label="Numero di Telefono">
          <el-input v-model="modCustomer.phone"></el-input>
        </el-form-item>
        <el-form-item label="Indirizzo E-mail">
          <el-input v-model="modCustomer.mail"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="modalModificaCliente = false">Annulla</el-button>
        <el-button type="primary" @click="updateCustomers(modCustomer.id)"
          >Salva</el-button
        >
      </span>
    </el-dialog>

    <el-dialog :visible.sync="modalCustomerPassword" append-to-body width="60%">
      <el-row type="flex" :gutter="10">
        <el-col :span="24">
          <div>
            <el-card
              v-for="item in customerPassword"
              :key="item.id"
              class="box-card"
            >
              <h1>{{ item.type }}</h1>
              <p class="testo-dialog-customer-password">
                <strong>Url:</strong> {{ item.url }}
              </p>
              <br />
              <p class="testo-dialog-customer-password">
                <strong>Host:</strong> {{ item.host }}
              </p>
              <br />
              <p class="testo-dialog-customer-password">
                <strong>Username:</strong>{{ item.username }}
              </p>
              <br />
              <p class="testo-dialog-customer-password">
                <strong>Password:</strong> {{ item.password }}
              </p>
              <br />
              <p class="testo-dialog-customer-password">
                <strong>Informazioni:</strong>{{ item.informations }}
              </p>
              <div style="width: 100%; text-align: right;">
                <el-button
                  type="primary"
                  icon="el-icon-edit"
                  circle
                ></el-button>
                <el-button
                  type="danger"
                  icon="el-icon-delete"
                  circle
                ></el-button>
              </div>
            </el-card>
          </div>
        </el-col>
      </el-row>
    </el-dialog>

    <el-row :gutter="10">
      <el-table :data="clienti" style="width: 100%">
        <el-table-column prop="company" label="Azienda" sortable>
        </el-table-column>
        <el-table-column prop="ref_name" label="Referente" sortable>
        </el-table-column>
        <el-table-column prop="phone" label="numero di telefono" sortable>
        </el-table-column>
        <el-table-column prop="mail" label="indirizzo e-mail" sortable>
        </el-table-column>
        <el-table-column align="right">
          <!-- <template slot="header" slot-scope="scope">
            <el-input v-model="search" placeholder="Cerca cliente" />
          </template> -->
          <template slot-scope="scope">
            <el-tooltip content="Visualizza Accessi" placement="top">
              <el-button
                circle
                icon="el-icon-lock"
                @click="visualizzaDatiCliente(scope.row.id)"
              ></el-button>
            </el-tooltip>
            <el-tooltip content="Modifica" placement="top">
              <el-button
                circle
                icon="el-icon-edit"
                type="primary"
                @click="modifyCustomer(scope.row.id)"
              ></el-button>
            </el-tooltip>
            <el-tooltip content="Elimina Cliente" placement="top">
              <el-button
                circle
                type="danger"
                icon="el-icon-delete"
                @click="openConfirmDelete(scope.row.id)"
              ></el-button>
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>
    </el-row>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "ListaClienti",
  data() {
    return {
      clienti: [],
      modalInserisciCliente: false,
      modalModificaCliente: false,
      modalCustomerPassword: false,
      company: null,
      ref_name: null,
      phone: null,
      mail: null,
      modCustomer: [],
      customerPassword: []
    };
  },
  created() {
    this.customers();
  },
  methods: {
    customers() {
      axios
        .get("customers")
        .then(response => {
          this.clienti = response.data;
        })
        .catch(error => {
          return error;
        });
    },
    inserisciCliente() {
      axios
        .post(
          "customers" +
            "?ref_name=" +
            this.ref_name +
            "&company=" +
            this.company +
            "&phone=" +
            this.phone +
            "&mail=" +
            this.mail,
          {}
        )
        .then(response => {
          this.modalInserisciCliente = false;
          this.works();
          return response;
        })
        .catch(error => {
          this.modalInserisciCliente = false;
          this.works();
          return error;
        });
    },
    modifyCustomer(id) {
      axios
        .get("customers/" + id)
        .then(response => {
          this.modCustomer = response.data;
          this.modalModificaCliente = true;
          return response;
        })
        .catch(error => {
          return error;
        });
    },
    updateCustomers(id) {
      axios
        .patch(
          "customers/" +
            id +
            "?company=" +
            this.modCustomer.company +
            "&ref_name=" +
            this.modCustomer.ref_name +
            "&phone=" +
            this.modCustomer.phone +
            "&mail=" +
            this.modCustomer.mail,
          {}
        )
        .then(response => {
          this.customers();
          this.modalModificaCliente = false;
          return response;
        })
        .catch(error => {
          this.modalModificaLavoro = false;
          return error;
        });
    },
    deleteCustomer(id) {
      axios
        .delete("customers/" + id)
        .then(response => {
          this.customers();
          return response;
        })
        .catch(error => {
          return error;
        });
    },
    openConfirmDelete(id) {
      this.$confirm("Sicuro di voler eliminare il cliente ?", "Attenzione", {
        confirmButtonText: "OK",
        cancelButtonText: "Annulla",
        type: "warning"
      })
        .then(() => {
          this.deleteCustomer(id);
        })
        .catch(() => {});
    },
    visualizzaDatiCliente(id) {
      axios
        .get("customerpassword/" + id)
        .then(response => {
          this.customerPassword = response.data;
          this.modalCustomerPassword = true;
          return response;
        })
        .catch(error => {
          return error;
        });
    }
  }
};
</script>
<style scoped>
.el-row {
  margin-bottom: 20px;
}
.testo-dialog-customer-password {
  font-size: 18px;
}
</style>
