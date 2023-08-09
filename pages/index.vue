<template>
  <div id="teste">
    <!-- CABEÇALHO -->
    <div>
      <v-row class="align-center">
        <v-col>
          <v-img src="logotipo.png" max-width="170"></v-img>
        </v-col>
        <v-col cols="3">
          <v-menu v-model="menu" :close-on-content-click="false" transition="scale-transition" offset-y max-width="290px"
            min-width="auto">
            <template v-slot:activator="{ on, attrs }">
              <v-text-field dense v-model="computedDateFormatted" label="Data" persistent-hint prepend-icon="mdi-calendar"
                readonly v-bind="attrs" v-on="on" outlined></v-text-field>
            </template>
            <v-date-picker v-model="date" no-title @input="menu = false" locale="Brazil"></v-date-picker>
          </v-menu>
        </v-col>
        <v-col class="text-right">
          <div>
            <p style="font-size: 12px;"><b style="font-size: 20px;">RECIBO DE COMPRA</b>
              <br>Rua Padre Estevão Pernet 718, Sala 2010
              <br>Bairro: Vila Gomes Cardim - São Paulo – SP
              <br>CEP: 03315-000 - CNPJ: 29.179.301/0001-06
            </p>
          </div>
        </v-col>
      </v-row>
    </div>
    <hr>
    <div class="mt-5">
      <v-row>
        <v-col><v-text-field dense label="Para" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="Elaborador" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="Cidade" outlined></v-text-field></v-col>
      </v-row>
    </div>
    <hr>
    <div class="mt-5">
      <v-data-table :headers="headers" :items="desserts" sort-by="calories" class="elevation-5" hide-default-footer>
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title class="mt-6"><v-text-field dense label="Recibo nº" outlined></v-text-field></v-toolbar-title>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <template v-slot:activator="{ on, attrs }">
                <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                  Novo Item
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="text-h5">{{ formTitle }}</span>
                </v-card-title>
                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedItem.item" label="Item"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedItem.codigo" label="Código"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedItem.descricao" label="Descrição"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedItem.quantidade" label="Quantidade"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedItem.unidade" label="Unidade"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedItem.preco" label="Preço"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="4">
                        <v-text-field v-model="editedItem.subtotal" label="Subtotal"></v-text-field>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="close">
                    Cancelar
                  </v-btn>
                  <v-btn color="blue darken-1" text @click="save">
                    Salvar
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-dialog v-model="dialogDelete" max-width="500px">
              <v-card>
                <v-card-title class="text-h5">Tem certeza que deseja deletar o item?</v-card-title>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="closeDelete">Cancelar</v-btn>
                  <v-btn color="blue darken-1" text @click="deleteItemConfirm">Deletar</v-btn>
                  <v-spacer></v-spacer>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deleteItem(item)">
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
      <v-row class="mt-3">
        <v-col><v-text-field dense label="Subtotal" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="Desconto" outlined color="red"></v-text-field></v-col>
        <v-col><v-text-field dense label="Imposto" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="Frete" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="Total" outlined></v-text-field></v-col>
      </v-row>
    </div>
    <hr>
    <div>
      <v-row class="mt-3">
        <v-col><v-text-field dense label="Nome" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="CPF" outlined></v-text-field></v-col>
      </v-row>
      <v-row style="margin-top: -40px;">
        <v-col><v-text-field dense label="Telefone" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="Endereço" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="Bairro" outlined></v-text-field></v-col>
      </v-row>
      <v-row style="margin-top: -40px;">
        <v-col><v-text-field dense label="Cidade" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="CEP" outlined></v-text-field></v-col>
      </v-row>
    </div>
    <hr>
    <div class="mt-5">
      <v-row>
        <v-col><v-text-field dense label="Pagamento PIX" outlined></v-text-field></v-col>
        <v-col><v-text-field dense label="Parcelado Mastercard" outlined></v-text-field></v-col>
      </v-row>
    </div>
    <hr>
    <div class="mt-5">
      <v-row>
        <v-col><v-text-field dense label="Observações Gerais" outlined></v-text-field></v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data: (vm) => ({
    date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    dateFormatted: vm.formatDate(
      new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10)
    ),
    menu: false,
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: 'Item',
        align: 'start',
        sortable: false,
        value: 'item',
      },
      { text: 'Código', value: 'codigo' },
      { text: 'Descrição', value: 'descricao' },
      { text: 'Quantidade', value: 'quantidade' },
      { text: 'Unidade', value: 'unidade' },
      { text: 'Preço', value: 'preco' },
      { text: 'Subtotal', value: 'subtotal' },
      { text: 'Ação', value: 'actions', sortable: false },
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      item: '',
      codigo: '',
      descricao: '',
      quantidade: '',
      unidade: '',
      preco: '',
      subtotal: '',
    },
    defaultItem: {
      item: '',
      codigo: '',
      descricao: '',
      quantidade: '',
      unidade: '',
      preco: '',
      subtotal: '',
    },
  }),
  computed: {
    computedDateFormatted() {
      return this.formatDate(this.date)
    },
    formTitle() {
      return this.editedIndex === -1 ? 'Novo Item' : 'Editar Item'
    },
  },
  watch: {
    date(val) {
      this.dateFormatted = this.formatDate(this.date)
    },
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    },
  },

  methods: {
    formatDate(date) {
      if (!date) return null
      const [year, month, day] = date.split('-')
      return `${day}/${month}/${year}`
    },

    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>

<style>
#teste {
  margin-left: -10px;
  margin-right: -10px;
}
</style>
