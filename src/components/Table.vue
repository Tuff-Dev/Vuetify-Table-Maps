<template>
  <div class="table-container">
    <v-data-table
      :headers="headers"
      :items="items"
      :items-per-page="5"
      :dense="dense"
      class="elevation-2"
      :caption="tableCaption"
      :loading="loading"
      :loading-text="loadingText"
    >
      <!-- Template above the data table -->
      <template v-slot:top>
        <v-toolbar flat v-if="tableTitle">
          <v-toolbar-title>{{ tableTitle }}</v-toolbar-title>
        </v-toolbar>
      </template>

      <!-- Template in actions column -->
      <template v-slot:item.actions="{ item }">
        <span v-for="action in recordActions" :key="action.name">
          <v-tooltip bottom>
            <template v-slot:activator="{ on, attrs }">
              <v-icon
                :disabled="isActionDisabled(action, item)"
                small
                v-on="on"
                v-bind="attrs"
                class="mr-2"
                @click="action.callback(item)"
              >
                {{ action.icon }}
              </v-icon>
            </template>
            <span>{{ action.tooltipText }}</span>
          </v-tooltip>
        </span>
      </template>

      <!-- Template for where no data is returned -->
      <template v-slot:no-data>
        {{ noRecordsMessage }}
      </template>
    </v-data-table>
  </div>
</template>

<script>
export default {
  name: "custom-table",
  props: {
    tableTitle: {
      type: String,
      required: false,
    },
    headers: {
      type: Array,
      required: true,
    },
    items: {
      type: Array,
      required: true,
    },
    recordActions: {
      type: Array,
      required: false,
    },
    dense: {
      type: Boolean,
      default: true,
    },
    noRecordsMessage: {
      type: String,
      default: "No data available",
    },
    tableCaption: {
      type: String,
      required: false,
    },
    loading: {
      type: Boolean,
      default: false,
    },
    loadingText: {
      type: String,
      default: 'Loading...'
    },
  },
  data() {
    return {};
  },
  methods: {
    editItem(item) {
      console.log("Edit: " + item);
    },
    deleteItem(item) {
      console.log("Delete: " + item);
    },
    isActionDisabled(action, item) {
      if (action) {
        return action.isDisabled(item);
      }
      return false;
    },
  },
};
</script>

<style>
tbody tr:nth-of-type(even) {
  background-color: rgba(0, 0, 0, 0.03)
}
</style>
