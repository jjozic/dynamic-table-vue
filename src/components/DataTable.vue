<template>
  <v-data-table
    :headers="headers"
    :items="vegetables"
    :search="search"
    sort-by="sku"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Vegetables</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
         <v-text-field
          v-model="search"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
              Add
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col v-show="isNew" cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.sku"
                      label="SKU"
                      type="number"
                    ></v-text-field>
                  </v-col>
                  <v-col v-show="isNew" cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.name"
                      label="Name"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.expiryDate"
                      label="Best-Before Date"
                      type="date"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.inventory"
                      label="Inventory Level"
                      type="number"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close"> Cancel </v-btn>
              <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5"
              >Are you sure you want to delete this item?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete"
                >Cancel</v-btn
              >
              <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
      <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize"> Reset </v-btn>
    </template>
  </v-data-table>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    search: '',
    headers: [
      { text: 'SKU', value: 'sku' },
      { text: 'Name', value: 'name' },
      { text: 'Best-Before Date', value: 'expiryDate' },
      { text: 'Inventory Level', value: 'inventory' },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
    vegetables: [],
    editedIndex: -1,
    editedItem: {
      sku: 0,
      name: '',
      expiryDate: new Date().toISOString().substr(0, 10),
      inventory: 0,
    },
    defaultItem: {
      sku: 0,
      name: '',
      expiryDate: new Date().toISOString().substr(0, 10),
      inventory: 0,
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item';
    },
    isNew() {
      return this.editedIndex === -1;
    },
  },

  watch: {
    dialog(val) {
      // eslint-disable-next-line no-unused-expressions
      val || this.close();
    },
    dialogDelete(val) {
      // eslint-disable-next-line no-unused-expressions
      val || this.closeDelete();
    },
  },
  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.vegetables = [
        {
          sku: 1234,
          name: 'Carrots',
          expiryDate: '2021-12-20',
          inventory: 240,
        },
        {
          sku: 4567,
          name: 'Potatoes',
          expiryDate: '2021-10-20',
          inventory: 140,
        },
        {
          sku: 4321,
          name: 'Corn',
          expiryDate: '2021-08-20',
          inventory: 40,
        },
        {
          sku: 2416,
          name: 'Cucumber',
          expiryDate: '2021-07-16',
          inventory: 80,
        },
      ];
    },

    editItem(item) {
      this.editedIndex = this.vegetables.indexOf(item);
      this.editedItem = { ...item };
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.vegetables.indexOf(item);
      this.editedItem = { ...item };
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.vegetables.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = { ...this.defaultItem };
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = { ...this.defaultItem };
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.vegetables[this.editedIndex], this.editedItem);
      } else {
        this.vegetables.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>
