<template>
  <v-container class="container">
    <ticket-details
      v-if="viewingDetails"
      :id="selectedTicket"
      @close="viewingDetails = false"
      class="detail"
    />
    <div class="row">
      <div class="col-8">
        <h3>List of Occurrences</h3>
      </div>
      <div class="col-4">
        <v-btn class="float-right" @click="toggleFilter">{{ filterOpen ? "Show All" : "Show Open" }}</v-btn>
      </div>
    </div>

    <v-data-table
      :headers="headers"
      :items="filteredOccurrences"
      :loading="loading"
      class="elevation-1"
    >
      <template v-slot:item.id="{ item }">
        {{ item.id }}
      </template>
      <template v-slot:item.policyNumber="{ item }">
        {{ item.policyNumber }}
      </template>
      <template v-slot:item.productDescription="{ item }">
        {{ item.productDescription }}
      </template>
      <template v-slot:item.covers="{ item }">
        <v-chip v-for="cover in item.covers" :key="cover" class="ma-2">
          {{ cover }}
        </v-chip>
      </template>
      <template v-slot:item.occurrenceState="{ item }">
        {{ item.occurrenceState }}
      </template>
      <template v-slot:item.actions="{ item }">
        <v-btn v-if="item.occurrenceState === 'in_repair'" @click="viewTicket(item)"
          >Repair</v-btn
        >
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
import TicketDetails from "./details.vue";
export default {
  components: {
    TicketDetails,
  },
  data() {
    return {
      viewingDetails: false,
      selectedTicket: {},
      loading: false,
      filterOpen: true,
      occurrences: [],
      headers: [
        { text: "Policy Number", value: "policyNumber" },
        { text: "Description", value: "description" },
        { text: "Status", value: "occurrenceState" },
        { text: "Actions", value: "actions" },
      ],
    };
  },
  computed: {
    filteredOccurrences() {
      return this.filterOpen
        ? this.occurrences.filter((occurence) => occurence.occurrenceState === "in_repair")
        : this.occurrences;
    },
  },
  methods: {
    toggleFilter() {
      this.filterOpen = !this.filterOpen;
    },
    viewTicket(item) {
      this.selectedTicket = item.id;
      this.viewingDetails = true;
    },
    async fetchOccurrencesForRepairman() {
      this.$axios
        .$get("/api/repairCompany/repair/" + this.$auth.user.nif, {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.occurrences = response;
        });
    }
  },
  mounted() {
    this.fetchOccurrencesForRepairman();
  }
};
</script>
<style scoped>
.container {
  max-width: none !important;
  width: 100% !important;
}
</style>
