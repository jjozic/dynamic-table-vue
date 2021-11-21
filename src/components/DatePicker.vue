<template>
  <v-menu
    ref="menu"
    v-model="menu"
    :close-on-content-click="false"
    :nudge-right="40"
    :return-value.sync="date"
    transition="scale-transition"
    offset-y
    min-width="290px"
  >
    <template v-slot:activator="{ on }">
      <v-text-field
        :value="value"
        @input="$emit('input', $event.target.value)"
        :label="label"
        readonly
        v-on="on"
      ></v-text-field>
    </template>
    <v-date-picker
      :value="value"
      @input="$emit('input', $event.target.value)"
      no-title
      scrollable
    >
      <v-spacer></v-spacer>
      <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
      <v-btn text color="primary" @click="$refs.menu.save(date)">OK</v-btn>
    </v-date-picker>
  </v-menu>
</template>

<script>
export default {
  data: () => ({
    // date: new Date().toISOString().substr(0, 10),
    menu: false,
    modal: false,
    menu2: false,
  }),
  props: ['label', 'value'],
};
</script>
