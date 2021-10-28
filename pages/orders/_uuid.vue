<template>
  <v-container>
    <v-card>
      <v-card-text>
        <div>{{ order.history[0].date }}</div>
        <p class="text-h5 text--primary">Origin: {{ order.origin }}</p>
        <p class="text-h5 text--primary">
          Destination: {{ order.destination }}
        </p>
        <p>Estimated: {{ order.estimated }}</p>
        <div class="text--primary">
          {{ order.desc }}
        </div>
      </v-card-text>
    </v-card>
    <br/>
    <v-timeline>
      <v-timeline-item v-for="(step, index) in order.history" :key="index">
        <span slot="opposite">{{step.date}}</span>
        <v-card class="elevation-2">
          <v-card-title class="text-h5"> {{step.location}} </v-card-title>
          <v-card-text>
            {{step.desc}}
          </v-card-text>
        </v-card>
      </v-timeline-item>
    </v-timeline>
  </v-container>
</template>

<script>
import config from "@/assets/config";

export default {
  data() {
    return {
      showHistory: false,
    };
  },

  async asyncData({ params, $axios }) {
    const uuid = params.uuid;
    let { info } = await $axios.$get(`${config.apiUrl}/order/${uuid}`);
    return {
      order: info,
    };
  },
};
</script>