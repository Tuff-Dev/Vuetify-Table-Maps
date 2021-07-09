<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>
      <v-container
          class="px-0"
          fluid
      >
        <v-switch
            v-model="darkMode"
            label="Dark Mode"
        ></v-switch>
      </v-container>
      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <CustomTable
          :headers="headers"
          :items="desserts"
          :record-actions="actions"
          table-title="Desserts">
        <template v-slot:header>
          <h1>Here might be a page title</h1>
        </template>
      </CustomTable>
    </v-main>
  </v-app>
</template>

<script>
import CustomTable from "./components/Table";

export default {
  name: 'App',

  components: {
    CustomTable,
  },

  data: () => ({
    darkMode: false,
    headers: [
      {
        text: 'Dessert (100g serving)',
        align: 'start',
        sortable: false,
        value: 'name',
      },
      { text: 'Calories', value: 'calories' },
      { text: 'Fat (g)', value: 'fat' },
      { text: 'Carbs (g)', value: 'carbs' },
      { text: 'Protein (g)', value: 'protein' },
      { text: 'Iron (%)', value: 'iron' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
    desserts: [
      {
        name: 'Frozen Yogurt',
        calories: 159,
        fat: 6.0,
        carbs: 24,
        protein: 4.0,
        iron: '1%',
      },
      {
        name: 'Ice cream sandwich',
        calories: 237,
        fat: 9.0,
        carbs: 37,
        protein: 4.3,
        iron: '1%',
      },
      {
        name: 'Eclair',
        calories: 262,
        fat: 16.0,
        carbs: 23,
        protein: 6.0,
        iron: '7%',
      },
      {
        name: 'Cupcake',
        calories: 305,
        fat: 3.7,
        carbs: 67,
        protein: 4.3,
        iron: '8%',
      },
      {
        name: 'Gingerbread',
        calories: 356,
        fat: 16.0,
        carbs: 49,
        protein: 3.9,
        iron: '16%',
      },
      {
        name: 'Jelly bean',
        calories: 375,
        fat: 0.0,
        carbs: 94,
        protein: 0.0,
        iron: '0%',
      },
      {
        name: 'Lollipop',
        calories: 392,
        fat: 0.2,
        carbs: 98,
        protein: 0,
        iron: '2%',
      },
      {
        name: 'Honeycomb',
        calories: 408,
        fat: 3.2,
        carbs: 87,
        protein: 6.5,
        iron: '45%',
      },
      {
        name: 'Donut',
        calories: 452,
        fat: 25.0,
        carbs: 51,
        protein: 4.9,
        iron: '22%',
      },
      {
        name: 'KitKat',
        calories: 518,
        fat: 26.0,
        carbs: 65,
        protein: 7,
        iron: '6%',
      },
    ],
    actions: [
      {
        name: 'Edit',
        callback: function (item) {
          console.log('Edit Item!')
          console.log(item)
        },
        isDisabled: function (item) {
          if (item.name === 'Ice cream sandwich') {
            return true
          }
        },
        icon: 'mdi-pencil',
        tooltipText: 'Edit Record',
      },
      {
        name: 'Delete',
        callback: function (item) {
          console.log('Delete Item!')
          console.log(item)
        },
        isDisabled: function (item) {
          if (item.name === 'Ice cream sandwich') {
            return true
          }
        },
        tooltipText: 'Delete',
        icon: 'mdi-delete',
      }
    ]
  }),
  watch: {
    darkMode: function () {
      console.log('Toggle Changed')
      this.$vuetify.theme.dark = this.darkMode
    }
  }
};
</script>
