<template>
  <v-card>
    <div class="row">
      <div class="col-9">
        <v-card-title>Set a service repair</v-card-title>
      </div>
      <div class="col-3">
        <v-card-actions class="float-right">
          <v-btn color="error" @click="closeView">Close</v-btn>
        </v-card-actions>
      </div>
    </div>
    <v-card-text>
      <v-col cols="12" sm="12" v-if="!showCustomServiceRepair">
        <v-select
          v-model="selectedServiceRepair"
          :items="serviceRepairsList"
          item-text="repairCompany"
          item-value="id"
          label="Select a service repair"
          persistent-hint
          hint="Please select a service repair"
          required
        ></v-select>
      </v-col>
      <v-col cols="12" sm="12" v-if="showCustomServiceRepair">
        <v-text-field v-model="customServiceRepair.name" label="Name of service repair" />
        <v-text-field
          v-model="customServiceRepair.email"
          label="Email of service repair"
        />
      </v-col>
      <v-btn @click="showCustomServiceRepair = !showCustomServiceRepair">{{
        (showCustomServiceRepair ? "Hide" : "Add") + " Custom Service Repair"
      }}</v-btn>
      <v-btn @click="submitSelectedStore">Submit</v-btn>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      selectedServiceRepair: null,
      customServiceRepair: { name: "", email: "" },
      serviceRepairsList: [],
      showCustomServiceRepair: false,
    };
  },
  methods: {
    submitSelectedStore() {
      this.$toast.clear();
      if (!this.selectedServiceRepair && !this.customServiceRepair) {
        this.$toast.error("Please select a service repair");
        return;
      }

      !this.showCustomServiceRepair
        ? this.setRepair(this.selectedServiceRepair)
        : this.setNewRepair(this.customServiceRepair);
    },
    closeView() {
      this.$emit("close");
    },
    async getAllRepair() {
      this.$axios
        .$get("/api/repairCompany", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.serviceRepairsList = response;
        });
    },
    async setRepair(serviceRepair) {
      this.$axios
        .$put(
          "/api/repairCompany/" + this.id,
          {
            repairCompanyID: serviceRepair,
          },
          {
            headers: {
              Accept: "application/json",
            },
          }
        )
        .then(() => {
          this.setOccurenceInRepair();
          // TODO
          // SEND EMAIL
        });
    },
    async setNewRepair() {
      this.$axios
        .$post(
          "/api/repairCompany",
          {
            repairCompany: this.customServiceRepair.name,
            email: this.customServiceRepair.email
          },
          {
            headers: {
              Accept: "application/json",
            },
          }
        )
        .then(() => {
          this.setOccurenceInRepair();
        });
    },
    async setOccurenceInRepair() {
      this.$axios
        .$put(
          "/api/occurrence/" + this.id,
          {
            occurrenceState: "repaired",
          },
          {
            headers: {
              Accept: "application/json",
            },
          }
        )
        .then(() => {
          this.$toast.success("Repair company set with success!", { duration: 3000 });
          this.closeView();
        });
    },
  },
  created() {
    this.getAllRepair();
  },
};
</script>
