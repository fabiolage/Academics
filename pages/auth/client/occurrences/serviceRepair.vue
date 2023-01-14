<template>
  <v-card>
    <div class="row">
      <div class="col-9">
        <v-card-title>Set a service repair</v-card-title>
      </div>
      <div class="col-3">
        <v-card-actions class="float-right">
          <v-btn color="error" @click="closeView()">Close</v-btn>
        </v-card-actions>
      </div>
    </div>
    <v-card-text>
      <v-col cols="12" sm="12">
        <v-select
          v-model="selectedServiceRepair"
          :items="serviceRepairsList"
          item-text="name"
          item-value="name"
          label="Select a service repair"
          persistent-hint
          hint="Please select a service repair"
          required
        ></v-select>
      </v-col>
      <v-col cols="12" sm="12">
        <v-text-field v-model="customServiceRepair" label="Or type the desired service repair" />
      </v-col>
      <v-btn @click="submitSelectedStore">Submit</v-btn>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      selectedServiceRepair: null,
      customServiceRepair: "",
      serviceRepairsList: [
        { name: "PCDiga" },
        { name: "Fnac" },
        { name: "Worten" },
      ],
    };
  },
  methods: {
    submitSelectedStore() {
      this.$toast.clear();
      if (!this.selectedServiceRepair && !this.customServiceRepair) {
        this.$toast.error("Please select a service repair");
        return;
      }
      const serviceRepair = !this.customServiceRepair ? this.selectedServiceRepair : this.customServiceRepair;
      // TODO
      // send axios request that sends serviceRepair
      console.log("Setting service repair to: "+serviceRepair);
    },
    closeView() {
      this.$emit('close')
    }
  },
};
</script>
