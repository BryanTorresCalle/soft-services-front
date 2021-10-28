<template>
<v-container>
  <v-card
    class="mx-auto orderCard"
    max-width="520"
    v-for="(order) in orders"
    v-bind:key="order[0]"
  >
    <v-card-text>
      <div>{{order[1].history[0].date}}</div>
      <p class="text-h5 text--primary">
        Origin: {{order[1].origin}}
      </p>
      <p class="text-h5 text--primary">
        Destination: {{order[1].destination}}
      </p>
      <p>Estimated: {{order[1].estimated}}</p>
      <div class="text--primary">
        {{order[1].desc}}
      </div>
    </v-card-text>
    <v-card-actions>
      <v-btn
        text
        color="teal accent-4"
        @click="gotoOrder(order[0])"
      >
        See more
      </v-btn>
    </v-card-actions>
  </v-card>
  <br/>
</v-container>
</template>

<script>
import config from "@/assets/config"

export default {
    data () {
      return {
        orders: [],
      }
    },

    beforeMount() {
      this.getData()
    },

    methods: {
      async getData() {
        let url = `${config.apiUrl}/orders`
        let {info} = await this.$axios.$get(url)
        this.orders = info
      },

      gotoOrder(uuid) {
        this.$router.push(`orders/${uuid}`)
      }
    }
}
</script>

<style>

.orderCard {
  margin-bottom: 1.3rem !important;
}

</style>