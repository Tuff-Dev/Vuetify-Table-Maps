<template>
  <div class="table-container">
    <v-data-table
      :headers="headers"
      :items="items"
      :dense="dense"
      :options="tableOptions"
      @pagination="optionsUpdated"
      class="elevation-2"
      :server-items-length="totalItems"
      :caption="tableCaption"
      :loading="loading"
      :loading-text="loadingText"
      :disable-pagination="externalPagination"
      :disable-filtering="externalFiltering"
      :disable-sort="externalSorting"
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
  name: "CustomTable",
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
    externalPagination: {
      type: Boolean,
      default: false
    },
    externalSorting: {
      type: Boolean,
      default: false
    },
    externalFiltering: {
      type: Boolean,
      default: false
    },
    tableOptions: {
      type: Object,
      default: function () {
        return {
          page: 1,
          itemsPerPage: 5,
          sortBy: [],
          sortDesc: [],
          groupBy: [],
          groupDesc: [],
          multiSort: false,
          mustSort: false,
        }
      }
    },
    totalItems: {
      type: Number,
      required: false
    }
  },
  data() {
    return {
    };
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
    pageUpdateFunction(pageNumber) {
      console.log('Page Updated')
      console.log(pageNumber)
    },
    optionsUpdated(options) {
      console.log(JSON.stringify(options))
    }
  },
};
</script>

<style>
tbody tr:nth-of-type(even) {
  background-color: rgba(0, 0, 0, 0.03)
}
</style>
