<template>
  <v-form v-model="valid">
    <v-container>
      <v-row>
        <v-col cols='12' md='12'>
            <h1>Create new order</h1>
        </v-col>
        <v-col cols="12" md="4">
          <v-text-field
            v-model="origin"
            label="Origin"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12" md="4">
          <v-text-field
            v-model="destination"
            label="Destination"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12" md="4">
            <v-menu
            ref='pickerMenu'
            v-model="menu"
            :close-on-content-click="false"
            :return-value.sync="estimated"
            transition="scale-transition"
            offset-y
            min-width="auto"
            >
                <template v-slot:activator="{ on, attrs }">
                    <v-text-field 
                    v-model="estimated" 
                    label="Estimated"
                    prepend-icon="mdi-calendar"
                    v-on="on"
                    v-bind='attrs'
                    ></v-text-field>
                </template>
                <v-date-picker
                v-model="estimated"
                no-title
                scrollable
                >
                <v-spacer></v-spacer>
                <v-btn
                text
                color="primary"
                @click="menu = false"
                >
                    Cancel
                </v-btn>
                <v-btn
                text
                color="primary"
                @click="$refs.pickerMenu.save(estimated)"
                >
                    OK
                </v-btn>
                </v-date-picker>
            </v-menu>
        </v-col>
        <v-col cols='12' md='12'>
            <v-textarea
            v-model="desc"
            label="Description for the order"
            ></v-textarea>
        </v-col>
        <v-col cols='12' md='12'>
        <!-- Bryk hace aqui adentro el componente-->
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>
<script>
export default {
    
    data: () => ({
        menu: false,
        valid: false,
        origin: '',
        destination: '',
        estimated: '',
        description: '',
        shopingList: ''
    })
};
</script>
