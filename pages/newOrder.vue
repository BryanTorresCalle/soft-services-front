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

        <v-col cols='12' md='6'>
          <v-text-field
            v-model="buyer"
            label="Buyer"
            required
          ></v-text-field>
        </v-col>

        <v-col cols='12' md='6'>
          <v-text-field
            v-model="supplier"
            label="Supplier"
            required
          ></v-text-field>
        </v-col>
      </v-row>

        
      <v-row
      v-for="(textfield, i) in textfields"
      :key="i"
      >
        <v-col cols='12' md='3'>
          <v-text-field
            v-model="textfield.item"
            label="Item"
            required></v-text-field>
        </v-col>

        <v-col cols='12' md='2'>
          <v-text-field
            v-model="textfield.amount"
            type="number"
            label="Amount"
            required></v-text-field>
        </v-col>

        <v-col cols='12' md='2'>
          <v-text-field
            v-model="textfield.weight"
            type="number"
            label="Weight"
            required></v-text-field>
        </v-col>

        <v-col cols='12' md='3'>
          <v-text-field
            v-model="textfield.size"
            type="number"
            label="Size"
            required></v-text-field>
        </v-col>

        <v-col cols='12' md='2'>
         <v-btn @click="remove(i)" style="margin: 10px 0 5px 0 " color="red"><v-icon>mdi-delete</v-icon> </v-btn>  
        </v-col>

        </v-row>


      <br>
      <v-btn @click="add" color="teal" ><v-icon dark>mdi-plus</v-icon></v-btn>  
      
    </v-container>
    <v-btn @click="createOrder" class="primary">Create Order</v-btn>
  </v-form>
</template>



<script>

import axios from "axios";
import config from "@/assets/config";
export default {
    
    data: () => ({
        menu: false,
        valid: false,
        origin: '',
        destination: '',
        estimated: '',
        desc: '',
        shopingList: '',
        buyer: '',
        supplier: '',
        textfields: []

    }),

    methods: {
      
     add () {
        this.textfields.push({ 
          item: "",
          amount: "",
          weight: "",
          size:"",
        })
        
     },
    
     remove (index) {
         this.textfields.splice(index, 1)
     },

     async createOrder () {

      let shoppingList = this.createShoppingList();

      var order = {
           origin: this.origin,
           destination: this.destination,
           desc: this.desc,
           items: shoppingList,
           };

      console.log(order);
       const URL = `${config.apiUrl}/orders`;
       const res = await axios.post(URL, order);
       console.log(res); 
      

       
     },

     async createShoppingList(){


       try {

        let items = [] 
         this.textfields.forEach(e => {
           items.push({
             item: e.item,
            amount: e.amount,
            weight: e.weight,
            size: e.size
           })
         });

         var formData = new FormData();
         var shoppingList = {
           listItems: items, 
           estimated: this.estimated,
           nameBuyer: this.buyer,
           nameSupplier: this.supplier
           }
       /*formData.append('listItems', this.textfields);
       formData.append('date', this.estimated);
       formData.append('nameBuyer', this.buyer);
       formData.append('nameSupplier', this.supplier);*/

       console.log(shoppingList);

       const URL = `${config.apiUrl}/shopping_list`;
       
       
       const res = await axios.post(URL, shoppingList);
       console.log(res); 
       return shoppingList; 

         
       } catch (error) {
         console.error(error);
       }




       
       

     }

     
  }
};
</script>
